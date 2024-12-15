TYPEs
![[Pasted image 20241207143151.png]]

![[Pasted image 20241207143250.png]]
![[Pasted image 20241207143322.png]]


# Derive Proc Macro
![[Pasted image 20241206125917.png]]
# Function Like ProcMacro
![[Pasted image 20241206130006.png]]
# Attribute Like
![[Pasted image 20241206130240.png]]
 
## **What Are Procedural Macros?**

Procedural macros work by taking **Rust code as input**, transforming it, and generating new code. This happens at compile time. Unlike declarative macros (`macro_rules!`), procedural macros allow for more complex and fine-grained transformations using Rust's Abstract Syntax Tree (AST).

### **Types of Procedural Macros**

1. **Function-like macros**: Custom syntax that looks like a function call (e.g., `my_macro!()`).
2. **Derive macros**: Automatically implement traits for structs or enums (e.g., `#[derive(Serialize)]`).
3. **Attribute macros**: Apply custom behavior to functions, structs, or modules (e.g., `#[my_attribute]`).

---

## **Key Concepts and Libraries**

To write procedural macros, you'll need the following crates:

1. **`proc-macro`**: Provides the foundation for procedural macros.
2. **`syn`**: Helps parse Rust syntax into a manipulatable AST.
3. **`quote`**: Used to generate Rust code from the manipulated AST.

Add these dependencies to your `Cargo.toml`:

```toml
[lib]
proc-macro = true

[dependencies]
syn = "2.0"
quote = "1.0"
```

---
# Example 0: Printing AST
```rust
use proc_macro::TokenStream;
use syn::{parse_macro_input, DeriveInput};

#[proc_macro_derive(PrintAST)] //abstract Syntax Tree
pub fn derive(input: TokenStream) -> TokenStream {
    let ast = parse_macro_input!(input as DeriveInput);
    eprintln!("{:#?}", ast);

    TokenStream::new()
}
```
//main.rs
```rust
use pro_mac::PrintAST;

#[derive(PrintAST)]
struct Person {
    name: String,
    age: u8,
}

fn main() {}
```
//OUTPUT
```md
DeriveInput {
    attrs: [],
    vis: Visibility::Inherited,
    ident: Ident {
        ident: "Person",
        span: #0 bytes(51..57),
    },
    generics: Generics {
        lt_token: None,
        params: [],
        gt_token: None,
        where_clause: None,
    },
    data: Data::Struct {
        struct_token: Struct,
        fields: Fields::Named {
            brace_token: Brace,
            named: [
                Field {
                    attrs: [],
                    vis: Visibility::Inherited,
                    mutability: FieldMutability::None,
                    ident: Some(
                        Ident {
                            ident: "name",
                            span: #0 bytes(64..68),
                        },
                    ),
                    colon_token: Some(
                        Colon,
                    ),
                    ty: Type::Path {
                        qself: None,
                        path: Path {
                            leading_colon: None,
                            segments: [
                                PathSegment {
                                    ident: Ident {
                                        ident: "String",
                                        span: #0 bytes(70..76),
                                    },
                                    arguments: PathArguments::None,
                                },
                            ],
                        },
                    },
                },
                Comma,
                Field {
                    attrs: [],
                    vis: Visibility::Inherited,
                    mutability: FieldMutability::None,
                    ident: Some(
                        Ident {
                            ident: "age",
                            span: #0 bytes(82..85),
                        },
                    ),
                    colon_token: Some(
                        Colon,
                    ),
                    ty: Type::Path {
                        qself: None,
                        path: Path {
                            leading_colon: None,
                            segments: [
                                PathSegment {
                                    ident: Ident {
                                        ident: "u8",
                                        span: #0 bytes(87..89),
                                    },
                                    arguments: PathArguments::None,
                                },
                            ],
                        },
                    },
                },
                Comma,
            ],
        },
        semi_token: None,
    },
}
```
## **Example 1: Derive Macro**

Let’s create a custom derive macro to implement a `Hello` trait for structs.

### Step 1: Define the Trait

```rust
pub trait Hello {
    fn say_hello(&self);
}
```

### Step 2: Implement the Macro

Create a procedural macro in a library crate (e.g., `hello_macro`):

#### `src/lib.rs`

```rust
use proc_macro::TokenStream;
use quote::quote;
use syn;

#[proc_macro_derive(Hello)]
pub fn hello_derive(input: TokenStream) -> TokenStream {
    // Parse the input tokens into a syntax tree
    let ast = syn::parse_macro_input!(input as syn::DeriveInput);
    
    // Get the name of the struct
    let name = &ast.ident;

    // Generate the implementation
    let gen = quote! {
        impl Hello for #name {
            fn say_hello(&self) {
                println!("Hello from {}!", stringify!(#name));
            }
        }
    };

    // Convert the generated code into a TokenStream
    gen.into()
}
```

### Step 3: Use the Macro

In a separate crate, add the macro as a dependency:

#### `Cargo.toml`

```toml
hello_macro = { path = "../hello_macro" }
```

#### Code Example

```rust
use hello_macro::Hello;

#[derive(Hello)]
struct MyStruct;

fn main() {
    let instance = MyStruct;
    instance.say_hello(); // Prints: "Hello from MyStruct!"
}
```

---

## **Example 2: Attribute Macro**

Let’s create a macro to log the execution time of a function.

#### `src/lib.rs`

```rust
use proc_macro::TokenStream;
use quote::quote;
use syn::{parse_macro_input, ItemFn};

#[proc_macro_attribute]
pub fn log_time(_attr: TokenStream, item: TokenStream) -> TokenStream {
    // Parse the function
    let input = parse_macro_input!(item as ItemFn);
    let func_name = &input.sig.ident;
    let block = &input.block;

    // Generate the wrapped function
    let gen = quote! {
        fn #func_name() {
            let start = std::time::Instant::now();
            #block
            let duration = start.elapsed();
            println!("{} took {:?} to execute.", stringify!(#func_name), duration);
        }
    };

    gen.into()
}
```

#### Usage

```rust
use my_macro::log_time;

#[log_time]
fn my_function() {
    for _ in 0..1_000_000 {
        // Simulating work
    }
}

fn main() {
    my_function();
}
```

---

## **How It Works**

1. **Parsing the Input**: Procedural macros receive their input as a `TokenStream`. Libraries like `syn` help convert this stream into an AST.
2. **Generating Code**: Use the `quote` crate to produce new Rust code.
3. **Returning Code**: Convert the generated code back into a `TokenStream`.

---

## **Deep Dive into Key Crates**

### 1. **`syn`**

- Parses `TokenStream` into an AST.
- Provides utilities to inspect and modify Rust code.

Example: Parsing a struct's fields

```rust
let fields = match ast.data {
    syn::Data::Struct(data) => data.fields,
    _ => panic!("Only structs are supported!"),
};
```

### 2. **`quote`**

- Generates code using macros.
- Provides the `quote!` macro to create Rust code fragments.

Example: Generate a simple function

```rust
let gen = quote! {
    fn hello() {
        println!("Hello, world!");
    }
};
```

### 3. **`proc-macro`**

- Provides the `TokenStream` type for interacting with macros.

---

## **Debugging Procedural Macros**

Debugging macros can be tricky. Here are tips:

1. **Print the Generated Code**:
    
    ```rust
    println!("{}", gen);
    ```
    
    This helps you inspect the generated Rust code.
    
2. **Use `cargo expand`**: Install the `cargo-expand` tool to see macro-expanded code.
    
    ```bash
    cargo install cargo-expand
    cargo expand
    ```
    

---

## **Exercises**

1. Create a derive macro that implements `Default` for a struct with all fields set to their default values.
2. Write an attribute macro to validate that a function takes no arguments.
3. Implement a function-like macro to create a basic HTML template.

Would you like help with any of these exercises or a deeper dive into a specific aspect?