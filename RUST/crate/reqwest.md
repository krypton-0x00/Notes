```rust
use reqwest;

#[tokio::main]
async fn main(){
    let result = reqwest::get("https://jsonplaceholder.typicode.com/todos/1").await;
    println!("{:?}",result);
}


```
OUTPUT
```rust
Ok(Response { url: "https://jsonplaceholder.typicode.com/todos/1", 
status: 200,
headers: {"date": "Thu, 31 Oct 2024 23:25:06 GMT", "content-type": "application/json; charset=utf-8", "content-length": "83", "connection": "keep-alive", "report-to": "{\"group\":\"heroku-nel\",\"max_age\":3600,\"endpoints\":[{\"url\":\"https://nel.heroku.com/reports?ts=1729546811&sid=e11707d5-02a7-43ef-b45e-2cf4d2036f7d&s=Gsuc4KImp1dDu0fWCPcbWnovAw5WgSLcExVTprvdJrw%3D\"}]}", "reporting-endpoints": "heroku-nel=https://nel.heroku.com/reports?ts=1729546811&sid=e11707d5-02a7-43ef-b45e-2cf4d2036f7d&s=Gsuc4KImp1dDu0fWCPcbWnovAw5WgSLcExVTprvdJrw%3D", "nel": "{\"report_to\":\"heroku-nel\",\"max_age\":3600,\"success_fraction\":0.005,\"failure_fraction\":0.05,\"response_headers\":[\"Via\"]}", "x-powered-by": "Express", "x-ratelimit-limit": "1000", "x-ratelimit-remaining": "999", "x-ratelimit-reset": "1729546827", "vary": "Origin, Accept-Encoding", "access-control-allow-credentials": "true", "cache-control": "max-age=43200", "pragma": "no-cache", "expires": "-1", "x-content-type-options": "nosniff", "etag": "W/\"53-hfEnumeNh6YirfjyjaujcOPPT+s\"", "via": "1.1 vegur", "cf-cache-status": "HIT", "age": "6234", "accept-ranges": "bytes", "server": "cloudflare", "cf-ray": "8db767055cfe9ba3-SIN", "alt-svc": "h3=\":443\"; ma=86400", "server-timing": "cfL4;desc=\"?proto=TCP&rtt=164247&sent=5&recv=6&lost=0&retrans=0&sent_bytes=2827&recv_bytes=693&delivery_rate=19650&cwnd=248&unsent_bytes=0&cid=8895f1a641298d55&ts=210&x=0\""} })
```
TO Only get the body we can use .text() method but it returns a future so we need to await it
```rust
use reqwest;

#[tokio::main]
async fn main(){
    let result = reqwest::get("https://jsonplaceholder.typicode.com/todos/1").await.unwrap().text().await;
    println!("{:?}",result);
}


```

## Specifying content types as headers

Let’s add a few headers to our query:

- `content_type` and `accept` to confirm we want a response as JSON
- `authorization` to pass our Spotify account token

We can’t pass these headers to `reqwest::get` directly. Instead, we’ll need to reach for the `client` module to chain our headers together. Let’s refactor our previous query to use `client` first:
```rust

#[tokio::main]
async fn main() {
    let client = reqwest::Client::new();
    let response = client
        .get("https://api.spotify.com/v1/search")
        // confirm the request using send()
        .send()
        .await
        // the rest is the same!
        .unwrap()
        .text()
        .await;
    println!("{:?}", response);
}
```
Now that we have a `client`, we can add our header config, like so:
```rust
let response = client
    .get("https://api.spotify.com/v1/search")
    .header(AUTHORIZATION, "Bearer [AUTH_TOKEN]")
    .header(CONTENT_TYPE, "application/json")
    .header(ACCEPT, "application/json")
    .send()
    ...
```
`[AUTH_TOKEN]` is your account’s OAuth token ([grab one here](https://developer.spotify.com/console/get-search-item/?q=Muse&type=track&market=US&limit=5&offset=5&include_external=)). If we log our response now, we should see… a different error status!

Instead of a 401 authorization error, we should get an error 400 bad request. This is because we haven’t specified what we’re searching for yet, so let’s do that:
```rust
let url = format!(
    "https://api.spotify.com/v1/search?q={query}&type=track,artist",
    // go check out her latest album. It's 🔥
    query = "Little Simz"
);
// the rest is the same as before!
let client = reqwest::Client::new();
let response = client
    .get(url)
    .header(AUTHORIZATION, "Bearer [AUTH_TOKEN]")
    .header(CONTENT_TYPE, "application/json")
    .header(ACCEPT, "application/json")
    .send()
    .await
    .unwrap();
println!("Success! {:?}", response)
```
```
// 👉 Ok("{\n  \"artists\" : {\n    \"href\" : \"https://api.spotify.com/v1/search?query=Lil+Simz&type=artist&offset=0&limit=20\",\n    \"items\" : [ {\n      \"external_urls\"...
```