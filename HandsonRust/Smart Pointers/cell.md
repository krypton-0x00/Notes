Creating a **cell smart pointer** in Rust involves understanding how to encapsulate data and manage interior mutability safely. A `Cell` is a type of smart pointer provided by Rust’s standard library that allows mutable access to data even when it is behind an immutable reference.

We will build a simplified version of `std::cell::Cell` step by step.

---

### **Step 1: Understand the Concept of Interior Mutability**

Interior mutability allows you to modify data even when you only have an immutable reference to it. Normally, Rust’s borrowing rules prevent this, but types like `Cell` and `RefCell` provide controlled ways to bypass these rules.

- **`Cell`**: Allows you to get and set values without borrowing.
- Works because it avoids references and instead uses **copy semantics**.

---

### **Step 2: Plan the Structure**

We'll:

1. Define a `Cell<T>` struct that owns a value of type `T`.
2. Use `unsafe` internally to manage mutability.
3. Provide methods to:
    - Get a copy of the value (`get`).
    - Update the value (`set`).

---

### **Step 3: Start with the Basic Structure**

Define the struct with a private field.

```rust
pub struct Cell<T> {
    value: T,
}
```

---

### **Step 4: Implement the `new` Constructor**

The `new` method initializes a `Cell` with a given value.

```rust
impl<T> Cell<T> {
    pub fn new(value: T) -> Self {
        Self { value }
    }
}
```

---

### **Step 5: Implement `get`**

For `get`, we return a copy of the value. To achieve this safely, the type `T` must implement the `Copy` trait.

```rust
impl<T: Copy> Cell<T> {
    pub fn get(&self) -> T {
        self.value
    }
}
```

---

### **Step 6: Implement `set`**

The `set` method allows us to update the value. This requires using an `unsafe` block to bypass Rust's normal borrowing rules.

```rust
impl<T> Cell<T> {
    pub fn set(&self, value: T) {
        unsafe {
            let ptr = &self.value as *const T as *mut T;
            *ptr = value;
        }
    }
}
```

- `&self.value` gets an immutable reference.
- `as *const T as *mut T` casts it to a mutable pointer.
- `*ptr = value` assigns a new value.

---

### **Step 7: Add a Test**

Here’s a complete working example with a test:

```rust
pub struct Cell<T> {
    value: T,
}

impl<T> Cell<T> {
    pub fn new(value: T) -> Self {
        Self { value }
    }

    pub fn set(&self, value: T) {
        unsafe {
            let ptr = &self.value as *const T as *mut T;
            *ptr = value;
        }
    }
}

impl<T: Copy> Cell<T> {
    pub fn get(&self) -> T {
        self.value
    }
}

fn main() {
    let cell = Cell::new(5);
    println!("Initial value: {}", cell.get());
    cell.set(10);
    println!("Updated value: {}", cell.get());
}
```

---

### **How It Works**

- `get`: Uses the `Copy` trait to return a copy of the value.
- `set`: Uses `unsafe` code to modify the data behind an immutable reference.

---

### **Step 8: Add Safety and Usability Features**

The `std::cell::Cell` provides additional safety features to ensure correct usage. For example, you can:

1. Add compile-time checks (like ensuring `T` is `Copy` for all operations).
2. Support types with `Debug` for better usability.

```rust
impl<T: std::fmt::Debug> std::fmt::Debug for Cell<T> {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        f.debug_struct("Cell").field("value", &self.get()).finish()
    }
}
```

---

### **Step 9: Comparison to `RefCell`**

Unlike `Cell`, `RefCell` allows mutability for non-`Copy` types. However, it incurs runtime borrow checks, whereas `Cell` relies on `Copy` semantics for compile-time safety.

Would you like to explore implementing a `RefCell` next?