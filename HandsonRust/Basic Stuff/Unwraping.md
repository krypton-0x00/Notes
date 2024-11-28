```rust
fn hello(name: Option<&str>) -> String {
    let unwrapped = name.unwrap_or_default(); //if none it will still continue and will not panic like with unwarp() so its gonna give us the default for the string i.e ""
    format!("Hi {}", unwrapped)
}

fn main() {
    println!("{:?}", hello(None));
}
```
### unwrap_or(value) 
here we can specify default value
```rust
fn hello(name: Option<&str>) -> String {
    let unwrapped = name.unwrap_or("unknown name");
    format!("Hi {}", unwrapped)
}

fn main() {
    println!("{:?}", hello(None)); //Hi unknown name
}
```
### unwrap_or_else(fn) -> eval at runtime
same as above
```rust
 let unwrapped = name.unwrap_or_else( | | "unknown name");
```