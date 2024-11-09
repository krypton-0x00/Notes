### 3. **`tokio`** - Asynchronous Runtime

- **Purpose**: Powers asynchronous programming in Rust with an event-driven runtime for async I/O, networking, and timers.
```rust
use tokio::time::{sleep, Duration};

#[tokio::main]
async fn main() {
    println!("Start sleeping...");
    sleep(Duration::from_secs(3)).await;
    println!("Done sleeping!");
}

```