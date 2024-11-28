```rust
fn add(x: i32, y: i32) -> i32 {
    x + y
}

fn main() {
    add(1, 2);
}

#[cfg(test)]
mod tests {
    use super::*;
    #[test]
    fn it_works() {
        assert_eq!(add(12, 12), 24);
    }
}

```