```rust
clone -> Deep copy -> reloacates new heap data,
copy -> shallow copy -> implemented to u32 , i32 etc
```


# FROM and INTO
The **`From`** trait defines a way to convert a value **from** one type **into** another.
```rust
impl From<&str> for String {
    fn from(s: &str) -> Self {
        s.to_string()
    }
}

fn main() {
    let s: String = String::from("Hello, Rust!");
    println!("{}", s);
}

```
The **`Into`** trait is the counterpart to `From`. If a type implements `From<T>`, it automatically implements `Into<U>` for the same types, so there's no need to implement `Into` manually.
**Direction**: Converts a value _into_ another type.
```rust
fn main() {
    let s: String = "Hello, Rust!".into();
    println!("{}", s);
}

```

# EG: 2

```rust
fn main() {
    // Using From
    let my_val = MyType::from(42);
    println!("my_val: {}", my_val.value);

    // Using Into
    let my_val2: MyType = 42.into(); // Same as above, but more ergonomic
    println!("my_val2: {}", my_val2.value);
}

```