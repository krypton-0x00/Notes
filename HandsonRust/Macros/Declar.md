![[Pasted image 20241206140026.png]]
```rust
#[macro_export] //pub for macros
macro_rules! vector {
    () => {
        Vec::new()
    };
    //single expression
    ($element:expr) => {{
        let mut vs = Vec::new();
        vs.push($element);
        vs
    }};
    //repeated macro args
    ($($element:expr),*) => {{
        //0 or more comma sperated expressions
        let mut vs = Vec::new();
        $(vs.push($element);)*
        vs
    }};
}

#[test]
fn empty_vector() {
    let x: Vec<u32> = vector![];
    assert!(x.is_empty());
}
#[test]
fn single() {
    let x: Vec<u32> = vector![12];
    assert!(!x.is_empty());
    assert_eq!(x.len(), 1);
    assert_eq!(x[0], 12);
}
#[test]
fn double() {
    let x: Vec<u32> = vector![12, 1];
    assert!(!x.is_empty());
    assert_eq!(x.len(), 2);
    assert_eq!(x[0], 12);
    assert_eq!(x[1], 1)
}
```

# Multi Forms
```rust
macro_rules! vector {
//form1
    ()=>{
       Vec::new();
    };
//form2
    ($($x:expr),*) => {
        {
            let v = vec![$($x),*];
            v
        }
    };
}

fn main() {
    let vec = vector![1, 2, 3, 4, 4];
    vec.iter().for_each(|x| println!("{}", x));
    let v2: Vec<i32> = vector!();
}
```

# Creating a DSL using macros
```rust
macro_rules! sql_query {
    ($table:expr, $condition:expr) => {
        format!("SELECT * FROM {} WHERE {}", $table, $condition)
    };
}

fn main() {
    let query = sql_query!("users", "age > 30");
    println!("{}", query); // Outputs: SELECT * FROM users WHERE age > 30
}

```

# JSON Builder DSL
```rust
use serde_json::json;

macro_rules! json_dsl {
    ( $( $key:expr => $value:expr ),* ) => {
        json!({
            $( $key: $value ),*
        })
    };
}

fn main() {
    let data = json_dsl!(
        "name" => "Alice",
        "age" => 30,
        "is_admin" => true
    );

    println!("{}", data); // Outputs: {"name":"Alice","age":30,"is_admin":true}
}

```

# Test framework DSL 
```rust
macro_rules! test_case {
    ($name:ident, $body:block) => {
        #[test]
        fn $name() {
            $body
        }
    };
}

test_case!(example_test, {
    assert_eq!(2 + 2, 4);
});

```