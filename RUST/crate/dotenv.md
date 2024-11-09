```rust
use dotenv::dotenv;
use std::env;

fn main() {
    dotenv().ok();
    let api_key = env::var("API_KEY").expect("API_KEY must be set");
    println!("API Key: {}", api_key);
}

```