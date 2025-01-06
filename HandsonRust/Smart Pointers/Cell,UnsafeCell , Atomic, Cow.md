### **1. `Cell<T>`**

- A `Cell<T>` provides **interior mutability** for types that implement `Copy`.
- Unlike `RefCell<T>`, `Cell<T>` does not use runtime borrow checking but allows setting and getting values by value.

#### Key Features:

- Allows mutable access even when the `Cell<T>` is immutable.
- Works only with types that implement the `Copy` trait.

#### Example:

```rust
use std::cell::Cell;

fn main() {
    let data = Cell::new(5);
    println!("Initial value: {}", data.get());

    data.set(10); // Mutate even though `data` is immutable.
    println!("Updated value: {}", data.get());
}
```

---

### **2. `UnsafeCell<T>`**

- The core of Rust's interior mutability pattern.
- It is used internally by `Cell<T>` and `RefCell<T>`.
- Allows unsafe, unchecked mutable access to its contents, even through an immutable reference.

#### Key Features:

- Required for low-level unsafe programming.
- Unsafe to use directly; generally wrapped by higher-level abstractions.

#### Example:

```rust
use std::cell::UnsafeCell;

fn main() {
    let value = UnsafeCell::new(42);
    unsafe {
        *value.get() = 55; // Directly modify the value.
        println!("Updated value: {}", *value.get());
    }
}
```
---

### 3. `Atomic*` Types**

- A family of types (`AtomicUsize`, `AtomicBool`, etc.) for atomic operations on primitive data.
- Ensures thread safety without requiring locks.

#### Key Features:

- Low-level synchronization primitives.
- Used for counters, flags, or other shared data where locks aren't needed.

#### Example:

```rust
use std::sync::atomic::{AtomicUsize, Ordering};
use std::thread;

fn main() {
    let counter = AtomicUsize::new(0);

    let handles: Vec<_> = (0..10)
        .map(|_| {
            let counter_ref = &counter;
            thread::spawn(move || {
                for _ in 0..100 {
                    counter_ref.fetch_add(1, Ordering::SeqCst);
                }
            })
        })
        .collect();

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Counter: {}", counter.load(Ordering::SeqCst));
}
```

---

### **Comparison of Common Smart Pointers**

|**Smart Pointer**|**Ownership**|**Thread Safety**|**Mutability**|**Primary Use**|
|---|---|---|---|---|
|`Box<T>`|Sole|No|No|Single ownership of heap-allocated data.|
|`Rc<T>`|Shared|No|No|Multiple ownership in single-threaded.|
|`Arc<T>`|Shared|Yes|No|Multiple ownership in multi-threaded.|
|`RefCell<T>`|Sole|No|Yes (interior mutability)|Borrow checking at runtime.|
|`Mutex<T>`|Sole|Yes|Yes|Thread-safe mutual exclusion.|
|`RwLock<T>`|Sole|Yes|Yes (read-write concurrency)|Thread-safe read-write access.|
|`Cell<T>`|Sole|No|Yes (interior mutability)|Mutability for `Copy` types.|
|`Atomic*`|Sole|Yes|Yes|Low-level atomic operations.|
