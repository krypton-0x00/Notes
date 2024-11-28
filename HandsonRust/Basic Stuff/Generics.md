```rust
use std::ops::Add;

fn main() {}

fn double_it<T: Add<Output = T> + Copy>(x: T) -> T {
    x + x
}

#[cfg(test)]
mod tests {
    use super::*;
    #[test]
    fn it_works() {
        assert_eq!(double_it(1.2), 2.41);
    }
}
```