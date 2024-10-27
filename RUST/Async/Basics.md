Step 1: Declaring an `async` function
```rust
async fn say_hello() {
    println!("Hello, world!");
}

```
#### Step 2: Awaiting an async function

To run the async function, you need to `.await` it inside another async context or within a runtime:

```rust
#[tokio::main]
async fn main() {
    say_hello().await;
}

```
In this example:
- The `#[tokio::main]` macro sets up an async runtime using **Tokio**, a popular async runtime in Rust.
- `say_hello().await;` tells Rust to wait for the `say_hello()` future to complete before moving on.
#### Step 3: Handling async I/O
Let's add some async I/O to make things more interesting, like making an HTTP request. You'll need to include the `reqwest` crate for making HTTP requests:

1. Add dependencies in your `Cargo.toml`:
```toml
[dependencies]
tokio = { version = "1", features = ["full"] }
reqwest = { version = "0.11", features = ["json"] }

```
2. Then, you can write a function that makes an HTTP request:
```rust
use reqwest;

async fn fetch_data() -> Result<(), reqwest::Error> {
    let response = reqwest::get("https://jsonplaceholder.typicode.com/todos/1")
        .await?
        .text()
        .await?;
    
    println!("Response: {}", response);
    Ok(())
}

#[tokio::main]
async fn main() {
    if let Err(e) = fetch_data().await {
        println!("An error occurred: {}", e);
    }
}

```
The `.await?` syntax is used to await the future and handle any potential errors.

#### Step 4: Understanding `.await`
The `await` keyword allows Rust to pause the execution of the current function while it waits for the future to be ready. During this time, other tasks or operations can be performed in the background.
### Here's how it works:

- When you call an async function, it doesn’t execute immediately. Instead, it returns a `Future`, which represents the value that will eventually be produced.
- When you `await` the future, Rust pauses the current function, schedules other tasks to run, and then resumes this function once the future is ready.
- 
  #  Using `async-std`

`async-std` is another async runtime, similar to Tokio but with a simpler API.

To use `async-std`, add it to your `Cargo.toml`:
```toml
[dependencies]
async-std = "1.10"

```
```rust
use async_std::task;
use reqwest;

async fn fetch_data() -> Result<(), reqwest::Error> {
    let response = reqwest::get("https://jsonplaceholder.typicode.com/todos/1")
        .await?
        .text()
        .await?;
    
    println!("Response: {}", response);
    Ok(())
}

fn main() {
    task::block_on(fetch_data());
}

```
`task::block_on()` is used to block the main thread until the future completes. This is useful if you want to run your async function without needing an explicit runtime.
### More Advanced Concepts

1. **Concurrency**:
    
    - You can run multiple async tasks concurrently using `tokio::spawn` or `async_std::task::spawn`. This creates a new async task that runs in the background.
```rust
use tokio::time::{sleep, Duration};

#[tokio::main]
async fn main() {
    let handle = tokio::spawn(async {
        sleep(Duration::from_secs(2)).await;
        println!("Task 1 done");
    });

    let handle2 = tokio::spawn(async {
        sleep(Duration::from_secs(1)).await;
        println!("Task 2 done");
    });

    handle.await.unwrap();
    handle2.await.unwrap();
}

```