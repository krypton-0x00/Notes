## **Binding Variables**

Bind values to variables within patterns.
```rust
fn main() {
    let value = Some(10);

    match value {
        Some(x @ 10) => println!("Matched ten: {}", x),
        Some(x) => println!("Matched: {}", x),
        None => println!("Matched none"),
    }
}

```
# **Match Guards**
```rust
fn main() {
    let num = Some(4);

    match num {
        Some(x) if x > 5 => println!("Greater than five"),
        Some(x) => println!("Less than or equal to five: {}", x),
        None => println!("None"),
    }
}

```