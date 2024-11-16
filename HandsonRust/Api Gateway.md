In Rust, you can build an API gateway using frameworks like **Actix Web** or **Axum** for HTTP request handling and **reqwest** or **hyper** for making HTTP requests to backend services. Here’s a guide on building a basic API gateway in Rust:

### 1. **Set Up a Basic Rust Project**

First, create a new Rust project with `cargo`:

```bash
cargo new api_gateway
cd api_gateway
```

Add dependencies in `Cargo.toml` for `actix-web`, `reqwest`, and optionally `tokio` (for async support):

```toml
[dependencies]
actix-web = "4"
reqwest = { version = "0.11", features = ["json"] }
tokio = { version = "1", features = ["full"] }
serde = { version = "1.0", features = ["derive"] }
```

### 2. **Create the Gateway Server with Actix Web**

This gateway will accept requests on specific routes and forward them to the appropriate backend services. Here’s an example setup:

```rust
use actix_web::{web, App, HttpServer, HttpResponse, HttpRequest};
use reqwest::Client;
use std::env;

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    // Initialize HTTP client to reuse for outbound requests
    let client = Client::new();

    // Start the Actix Web server
    HttpServer::new(move || {
        App::new()
            .app_data(web::Data::new(client.clone()))
            .route("/service1/{tail:.*}", web::to(forward_service1))
            .route("/service2/{tail:.*}", web::to(forward_service2))
    })
    .bind("127.0.0.1:3000")?
    .run()
    .await
}

// Forward requests to Service 1
async fn forward_service1(
    client: web::Data<Client>,
    req: HttpRequest,
) -> HttpResponse {
    forward_request(client, req, "http://localhost:4001").await
}

// Forward requests to Service 2
async fn forward_service2(
    client: web::Data<Client>,
    req: HttpRequest,
) -> HttpResponse {
    forward_request(client, req, "http://localhost:4002").await
}

// Core forwarding function
async fn forward_request(
    client: web::Data<Client>,
    req: HttpRequest,
    target_base: &str,
) -> HttpResponse {
    let uri = format!("{}/{}", target_base, req.path());

    match client.get(&uri).send().await {
        Ok(resp) => HttpResponse::Ok().body(resp.text().await.unwrap_or_default()),
        Err(_) => HttpResponse::InternalServerError().body("Service unavailable"),
    }
}
```

### 3. **Explanation of the Gateway**

- **HTTP Routes**: We define routes `/service1/{tail:.*}` and `/service2/{tail:.*}`, which will forward requests to `localhost:4001` and `localhost:4002` respectively. `{tail:.*}` is a wildcard capturing the full path after `/service1` or `/service2`.
- **Forwarding Logic**: The `forward_request` function uses `reqwest` to forward requests to the target service URL. You can expand this to handle HTTP methods other than `GET` by adjusting `client.get(...)`.
- **Error Handling**: Basic error handling is added for unavailable services, returning a `500` response if the request to the backend fails.

### 4. **Running and Testing the Gateway**

Start backend services on `localhost:4001` and `localhost:4002` or replace these with actual services you want to proxy. Then run the gateway with:

```bash
cargo run
```

To test, send requests to the gateway:

```bash
curl http://localhost:3000/service1/your-endpoint
curl http://localhost:3000/service2/another-endpoint
```

### 5. **Extending the Gateway**

For a production-ready API gateway, you could:
- **Add Authentication**: Integrate JWT handling or an OAuth provider.
- **Logging and Metrics**: Use Actix middleware for logging and Prometheus for metrics.
- **Request/Response Transformation**: Modify the `forward_request` function to handle request transformations (e.g., adding headers) or response transformations (e.g., changing JSON structures).

This Rust-based gateway provides a foundation for routing requests to multiple services, allowing for additional middleware and enhancements based on project requirements.

# OR

```rust
// Import required dependencies from actix-web
use actix_web::{
    web, // For shared state and routing
    App, HttpResponse, HttpServer, Error, // Core actix components
    http::{header, StatusCode}, // HTTP utilities
    dev::ServiceRequest, // Access to underlying request details
};
use serde::{Deserialize, Serialize}; // For YAML serialization/deserialization
use std::{fs::File, io::Read}; // File operations
use awc::Client;  // Actix web client for making HTTP requests

// Structure representing a single service configuration
// #[derive] automatically implements the listed traits for our struct
#[derive(Debug, Deserialize, Serialize, Clone)]
pub struct ServiceConfig {
    pub path: String,        // The path prefix this service handles (e.g., "/users")
    pub target_service: String, // The actual service URL (e.g., "http://user-service")
    pub target_port: String,    // The port the service runs on
}

// Main configuration structure that holds all gateway settings
#[derive(Debug, Deserialize, Serialize, Clone)]
pub struct GatewayConfig {
    pub authorization_api_url: String,    // URL of the auth service
    pub services: Vec<ServiceConfig>,     // List of service configurations
}

// Function to load and parse the YAML configuration file
fn load_config(path: &str) -> GatewayConfig {
    // Open the configuration file
    let mut file = File::open(path).expect("Failed to open config file");
    
    // Read file contents to string
    let mut contents = String::new();
    file.read_to_string(&mut contents).expect("Failed to read config file");
    
    // Parse YAML string into GatewayConfig struct
    serde_yaml::from_str(&contents).expect("Failed to parse YAML")
}

// Function to check if a request is authorized
// Takes: 
// - ServiceRequest: The incoming request
// - auth_api_url: URL of the authorization service
// - client: HTTP client for making requests
async fn authorize_user(
    req: &ServiceRequest,
    auth_api_url: &str,
    client: &Client,
) -> Result<String, Error> {
    // Extract Authorization header from request
    // If not present, use empty string as default
    let auth_header = req
        .headers()
        .get(header::AUTHORIZATION)
        .and_then(|h| h.to_str().ok())
        .unwrap_or_default();

    // Forward authorization request to auth service
    let response = client
        .get(auth_api_url)
        .insert_header((header::AUTHORIZATION, auth_header))
        .send()
        .await
        // Convert network errors to ServiceUnavailable response
        .map_err(|_| HttpResponse::ServiceUnavailable().finish())?;

    // Check authorization response
    if response.status().is_success() {
        Ok(auth_header.to_string()) // Return auth token if successful
    } else {
        Err(HttpResponse::Unauthorized().into()) // Return error if unauthorized
    }
}

// Function to find the appropriate service config for a given path
// Returns Option<&ServiceConfig> - Some(config) if found, None if not found
fn get_service_config<'a>(path: &str, services: &'a [ServiceConfig]) -> Option<&'a ServiceConfig> {
    // Find first service whose path is a prefix of the requested path
    services.iter().find(|c| path.starts_with(&c.path))
}

// Main request handler - processes all incoming requests
// Takes:
// - ServiceRequest: The incoming request
// - config: Gateway configuration (shared application state)
// - client: HTTP client (shared application state)
async fn handle_request(
    req: ServiceRequest,
    config: web::Data<GatewayConfig>,
    client: web::Data<Client>,
) -> Result<HttpResponse, Error> {
    // Step 1: Extract path and find matching service
    let path = req.path();
    let service_config = match get_service_config(path, &config.services) {
        Some(config) => config,
        None => return Ok(HttpResponse::NotFound().finish()), // Return 404 if no service matches
    };

    // Step 2: Verify authorization
    let auth_token = authorize_user(&req, &config.authorization_api_url, &client)
        .await
        .map_err(|_| HttpResponse::Unauthorized().finish())?;

    // Step 3: Construct target URL for the downstream service
    let target_url = format!(
        "{}:{}{}",
        service_config.target_service, 
        service_config.target_port, 
        path
    );

    // Step 4: Prepare forwarded request
    // Create new request with same method and target URL
    let mut forwarded_req = client
        .request(req.method().clone(), &target_url)
        .insert_header((header::AUTHORIZATION, auth_token));

    // Step 5: Copy all original headers except Authorization
    // (we already handled Authorization separately)
    for (header_name, header_value) in req.headers() {
        if header_name != &header::AUTHORIZATION {
            forwarded_req = forwarded_req.insert_header((header_name.clone(), header_value.clone()));
        }
    }

    // Step 6: Forward request to downstream service
    let response = forwarded_req
        .send()
        .await
        .map_err(|_| HttpResponse::ServiceUnavailable().finish())?;

    // Step 7: Construct response to client
    // Start building response with same status code
    let mut builder = HttpResponse::build(response.status());
    
    // Copy all response headers
    for (header_name, header_value) in response.headers() {
        builder.insert_header((header_name.clone(), header_value.clone()));
    }

    // Return response with body
    Ok(builder.body(response.body().await.unwrap_or_default()))
}

// Main function - entry point of the application
#[actix_web::main] // Makes async main function possible
async fn main() -> std::io::Result<()> {
    // Step 1: Load configuration from file
    let config = web::Data::new(load_config("config.yaml"));
    
    // Step 2: Create shared HTTP client
    let client = web::Data::new(Client::default());

    // Step 3: Configure and start HTTP server
    HttpServer::new(move || {
        App::new()
            // Add shared state
            .app_data(config.clone())     // Share config across threads
            .app_data(client.clone())     // Share client across threads
            // Set up default service handler
            .default_service(web::route().to(handle_request))
    })
    .bind("0.0.0.0:8080")?  // Listen on all interfaces, port 8080
    .run()                  // Start server
    .await                  // Wait for server to finish
}

// Test module
#[cfg(test)]
mod tests {
    use super::*;
    use actix_web::{
        test,
        http::Method,
    };

    // Test service configuration matching
    #[actix_web::test]
    async fn test_get_service_config() {
        // Create test service configuration
        let services = vec![
            ServiceConfig {
                path: "/users".to_string(),
                target_service: "http://user-service".to_string(),
                target_port: "8080".to_string(),
            },
        ];

        // Test matching path
        let config = get_service_config("/users/123", &services);
        assert!(config.is_some());
        assert_eq!(config.unwrap().path, "/users");

        // Test non-matching path
        let config = get_service_config("/invalid", &services);
        assert!(config.is_none());
    }

    // Test 404 handling
    #[actix_web::test]
    async fn test_handle_request_not_found() {
        // Create test application
        let app = test::init_service(
            App::new()
                .app_data(web::Data::new(GatewayConfig {
                    authorization_api_url: "http://auth".to_string(),
                    services: vec![],
                }))
                .app_data(web::Data::new(Client::default()))
                .default_service(web::route().to(handle_request))
        ).await;

        // Create test request
        let req = test::TestRequest::with_uri("/unknown")
            .method(Method::GET)
            .to_request();

        // Verify response
        let resp = test::call_service(&app, req).await;
        assert_eq!(resp.status(), StatusCode::NOT_FOUND);
    }
}
```

config.yaml

```yaml
authorization_api_url: "https://auth.example.com/api/v1/authorization"


services:
  - path: "/users"
    target_service: "http://user-service.default.svc.cluster.local"
    target_port: "8080"


  - path: "/orders"
    target_service: "http://order-service.default.svc.cluster.local"
    target_port: "8080"
```