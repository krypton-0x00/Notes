```rust

struct WrappedNumber {
    value: i32,
    is_even: bool,
}

fn main() {
    let mut numbers = vec![];

    for number in 0..100 {
        numbers.push(number);
    }
    dbg!(&numbers);

    let wrapped_numbers: Vec<WrappedNumber> = numbers
        .iter()
        .map(|x| WrappedNumber {
            value: *x,
            is_even: x % 2 == 0,
        })
        .collect();
    dbg!(&wrapped_numbers);
}

```
# Filter
```rust
fn main() {
    let mut numbers = vec![];

    for number in 0..100 {
        numbers.push(number);
    }
    dbg!(&numbers);

    let wrapped_numbers: Vec<&i32> = numbers.iter().filter(|num| *num % 2 == 0).collect();

    dbg!(&wrapped_numbers);
}
```