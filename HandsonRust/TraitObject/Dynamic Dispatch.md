
## **What is Dynamic Dispatch?**

- **Trait objects (`dyn Trait`)**:
    - Dynamic dispatch is enabled via **trait objects**.
    - A trait object is a pointer (`Box`, `&`, or `Rc`) to an object and a table of function pointers (vtable) to its methods.
    - These pointers allow Rust to call the correct method implementation for the type stored inside the trait object at runtime.

---

## **How Dynamic Dispatch Works**

When you create a trait object (`Box<dyn Trait>`, `&dyn Trait`, or `Rc<dyn Trait>`), Rust:

1. Stores a pointer to the data (e.g., `Box<T>`, `&T`, or `Rc<T>`).
2. Attaches a **vtable** to the trait object. The vtable is a lookup table for the methods defined in the trait.

### **Illustration**

Imagine a trait `Shape` with the method `area`. When you call `shape.area()` on a `dyn Shape`, Rust:

- Looks up the correct `area` implementation in the vtable for the type stored in the trait object.
- Calls that method dynamically.

---

## **Defining and Using Trait Objects**

### **Example: Dynamic Dispatch with Trait Objects**

```rust
trait Shape {
    fn area(&self) -> f64;
}

struct Circle {
    radius: f64,
}

struct Rectangle {
    width: f64,
    height: f64,
}

impl Shape for Circle {
    fn area(&self) -> f64 {
        3.14 * self.radius * self.radius
    }
}

impl Shape for Rectangle {
    fn area(&self) -> f64 {
        self.width * self.height
    }
}

fn print_area(shape: &dyn Shape) {
    println!("Area: {}", shape.area());
}

fn main() {
    let circle = Circle { radius: 3.0 };
    let rectangle = Rectangle { width: 4.0, height: 5.0 };

    print_area(&circle);     // Calls Circle's implementation
    print_area(&rectangle);  // Calls Rectangle's implementation
}
```

### **Key Points**

1. `&dyn Shape` is a **trait object**.
2. The `area` method is resolved at runtime using the vtable.
3. You can store different types (`Circle`, `Rectangle`) behind a `dyn Shape`.

---

## **When to Use Dynamic Dispatch**

Dynamic dispatch is useful when:

- You need a **heterogeneous collection**:
    - A single container storing objects of different types implementing the same trait.
- Runtime polymorphism is required, and type-specific optimizations aren’t a concern.

---

## **Trait Objects vs. Static Dispatch**

|Feature|Static Dispatch|Dynamic Dispatch|
|---|---|---|
|Resolution Time|Compile time|Runtime|
|Performance|Faster, as method calls are inlined.|Slower due to vtable lookup.|
|Flexibility|Requires knowing the type at compile time.|Allows heterogeneous types.|
|Syntax|`impl Trait` or generic bounds|`dyn Trait`|

---

## **Dynamic Dispatch with Box**

Sometimes you don’t know the size of objects at compile time. Use `Box<dyn Trait>` to store dynamically sized types.

### **Example: Boxed Trait Objects**

```rust
fn main() {
    let circle = Circle { radius: 2.5 };
    let rectangle = Rectangle { width: 3.0, height: 4.0 };

    // Store both Circle and Rectangle in a Vec of Box<dyn Shape>
    let shapes: Vec<Box<dyn Shape>> = vec![
        Box::new(circle),
        Box::new(rectangle),
    ];

    for shape in shapes {
        println!("Area: {}", shape.area());
    }
}
```

### **Why `Box<dyn Shape>`?**

- `Circle` and `Rectangle` have different sizes.
- `Box<dyn Shape>` provides a way to store dynamically sized types in a homogeneous collection.

---

## **Object Safety and `dyn Trait`**

Not all traits can be used as trait objects. A trait is **object-safe** if:

1. It does not have methods that require knowing the concrete `Self` type.
2. It does not have generic methods.

### **Example of Object-Safe Trait**

```rust
trait ObjectSafe {
    fn action(&self); // Allowed
}
```

### **Example of Non-Object-Safe Trait**

```rust
trait NotObjectSafe {
    fn new() -> Self;  // Requires knowing `Self`.
    fn action<T>(&self, param: T); // Generic methods are not allowed.
}
```

---

## **Performance Implications**

1. **Vtable Lookup Overhead**:
    
    - Dynamic dispatch involves an indirection via a vtable, which is slightly slower than static dispatch.
    - In most cases, this overhead is negligible compared to the flexibility it provides.
2. **Inlining**:
    
    - Methods cannot be inlined with dynamic dispatch.
    - If performance is critical, prefer static dispatch (`impl Trait` or generics).

---

## **Dynamic Dispatch in a GUI Example**

Dynamic dispatch is often used in scenarios like building a GUI, where you need to handle multiple types of widgets.

```rust
trait Draw {
    fn draw(&self);
}

struct Button {
    label: String,
}

struct TextBox {
    text: String,
}

impl Draw for Button {
    fn draw(&self) {
        println!("Drawing Button: {}", self.label);
    }
}

impl Draw for TextBox {
    fn draw(&self) {
        println!("Drawing TextBox: {}", self.text);
    }
}

struct Screen {
    components: Vec<Box<dyn Draw>>,
}

impl Screen {
    fn run(&self) {
        for component in &self.components {
            component.draw();
        }
    }
}

fn main() {
    let button = Button { label: "Submit".to_string() };
    let text_box = TextBox { text: "Enter text".to_string() };

    let screen = Screen {
        components: vec![
            Box::new(button),
            Box::new(text_box),
        ],
    };

    screen.run();
}
```

- `Vec<Box<dyn Draw>>` stores components like `Button` and `TextBox`.
- Each component’s `draw` method is called dynamically at runtime.
