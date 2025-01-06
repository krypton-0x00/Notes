# RC
#### Where to use RC?
```rust
struct Node {  
    value:  i32,  
    next:  Option<Box<Node>>,  
}  
fn main() {  
    let a = Node{value: 1, next: None};  
    let b = Node{value: 2, next: Some(Box::new(a))};  
    let c = Node{value: 3, next: Some(Box::new(a))}; //ERROR: Value used after being move  
}
```

RC
```rust
struct Node {  
    value:  i32,  
    next:  Option<Rc<Node>>,  
}  
fn main() {  
    let a = Node{value: 1, next: None};  
    let b = Node{value: 2, next: Some(Rc::clone(&a))};  
    let c = Node{value: 3, next: Some(Rc::clone(&a))};  
}
```
![[Pasted image 20250105182325.png]]


# RefCell

### What is Interior Mutability?

Interior mutability allows you to mutate data even when you only have an immutable reference to it. This is achieved through types like `RefCell` and `Mutex`, which enforce borrowing rules **at runtime** instead of compile time.

The most common tool for interior mutability in single-threaded contexts is `RefCell`.

---

### What is `RefCell`?

`RefCell` is a type provided by Rust in the `std::cell` module. It provides interior mutability by dynamically enforcing Rust's borrowing rules at **runtime**.

1. **Immutable Borrow (`.borrow()`)**:
    
    - Returns an immutable reference to the inner value.
    - Panics if there's already a mutable borrow.
2. **Mutable Borrow (`.borrow_mut()`)**:
    
    - Returns a mutable reference to the inner value.
    - Panics if there are existing immutable or mutable borrows.

---

### How `RefCell` Works

At its core, `RefCell` keeps track of the borrow state of its inner value:

- **No Borrows**: Either mutable or immutable borrow is allowed.
- **Immutable Borrows**: Any number of immutable borrows is allowed, but no mutable borrow.
- **Mutable Borrow**: Only one mutable borrow is allowed, and no immutable borrows.

If you break these rules, your program will **panic** at runtime.


PROBLEM 
```rust
pub trait Messagner{  
    fn send(&self,msg:&str);  
}  
  
struct MockMessenger{  
    messages: Vec<String>  
}  
impl Messagner for MockMessenger{  
    fn send(&self, msg:&str){  
        &self.messages.push(msg.to_string()); //ERROR: Cannot borrow immutable local variable `self. messages` as mutable  
    }  
}
```

SOLUTION USING `RefCell<T>`
```rust
use std::cell::RefCell;  
  
pub trait Messagner{  
    fn send(&self,msg:&str);  
}  
  
struct MockMessenger{  
    messages: RefCell<Vec<String>>  
}  
impl Messagner for MockMessenger{  
    fn send(&self, msg:&str){  
        &self.messages.borrow_mut().push(msg.to_string());  
    }  
}
```
### Having Interior Mutability on shared data using Rc with RefCell
```rust
use std::cell::RefCell;  
use std::ops::DerefMut;  
use std::rc::Rc;  
  
  
  
fn main() {  
    let a = Rc::new(RefCell::new(5));  
  
  
    let b = Rc::clone(&a);  
    let c = Rc::clone(&b);  
  
    let mut value = b.borrow_mut();  
    *value += 10;  
    println!("value: {}", value);  
}
```

# Memory Leaks with Rc and RefCell
Reference cycles in Rust occur when two or more `Rc<T>` smart pointers refer to each other directly or indirectly, preventing the reference count from ever reaching zero. This creates a memory leak because the cyclic references can never be deallocated, even if they are no longer accessible.

LEAK
```rust
use std::rc::Rc;  
use std::cell::RefCell;  
  
#[derive(Debug)]  
struct Node {  
    value: i32,  
    next: RefCell<Option<Rc<Node>>>,  
}  
  
fn main() {  
    let node1 = Rc::new(Node {  
        value: 1,  
        next: RefCell::new(None),  
    });  
  
    let node2 = Rc::new(Node {  
        value: 2,  
        next: RefCell::new(Some(Rc::clone(&node1))),  
    });  
  
    // Create a cycle: node1 points back to node2  
    *node1.next.borrow_mut() = Some(Rc::clone(&node2));  
  
    println!("Node1: {:?}", node1);  
    println!("Node2: {:?}", node2);  
  
    // Even though both nodes are out of scope, the memory is not freed.  
}
```
![[Pasted image 20250105195235.png]]


# Preventing Reference Cycles using weak pointer.
```rust
use std::rc::{Rc, Weak};
use std::cell::RefCell;

#[derive(Debug)]
struct Node {
    value: i32,
    next: RefCell<Option<Rc<Node>>>,
    prev: RefCell<Option<Weak<Node>>>, // Use Weak to avoid cycle
}

fn main() {
    let node1 = Rc::new(Node {
        value: 1,
        next: RefCell::new(None),
        prev: RefCell::new(None),
    });

    let node2 = Rc::new(Node {
        value: 2,
        next: RefCell::new(None),
        prev: RefCell::new(Some(Rc::downgrade(&node1))), // Weak reference to node1
    });

    *node1.next.borrow_mut() = Some(Rc::clone(&node2)); // node1 points to node2



    println!("n1 strong count: {}", Rc::strong_count(&node1)); // 1
    println!("n2 strong count: {}", Rc::strong_count(&node2)); // 1

    println!("node2 weak count: {}", Rc::weak_count(&node2)); // 1 (node1's weak pointer)
}

```

![[Pasted image 20250105201548.png]]

### What is a Weak Pointer?

A `Weak<T>` pointer:

- Points to data managed by an `Rc<T>`, but **does not increase the reference count**.
- Is useful for breaking **reference cycles**, where two or more `Rc<T>` instances point to each other and keep the memory alive indefinitely.

Think of `Weak<T>` as a way of saying:

> "I want to observe the data in this `Rc<T>`, but I don't own it, and I'm okay if it gets deleted."

Example where to use it:
```rs
parent -> child
child -> parent

```
If both pointers are `Rc<T>`, neither can be dropped because they keep incrementing each other’s reference count. A **memory leak** happens.


### Upgrading a `Weak<T>`

A `Weak<T>` pointer doesn’t own the data, so to access it, you must "upgrade" it to an `Rc<T>`. If the original `Rc<T>` is dropped, upgrading a `Weak<T>` will return `None`.

```rust
use std::rc::{Rc, Weak};

fn main() {
    let strong = Rc::new(5);
    let weak = Rc::downgrade(&strong);

    // Upgrade the weak pointer to access the value
    if let Some(shared) = weak.upgrade() {
        println!("Strong value: {}", *shared); // Outputs 5
    } else {
        println!("Value is no longer available");
    }

    // Drop the strong reference
    drop(strong);

    // Upgrading now fails
    if let Some(shared) = weak.upgrade() {
        println!("Strong value: {}", *shared);
    } else {
        println!("Value is no longer available"); // Outputs this
    }
}

```



### Key Differences Between `Rc<T>` and `Weak<T>`

|Feature|`Rc<T>`|`Weak<T>`|
|---|---|---|
|Ownership|Owns the value|Does not own the value|
|Increases Count|Yes (strong count)|No|
|Prevents Dropping|Yes|No|
|Usage|Strong reference for shared ownership|Non-owning reference to avoid cycles|
### **Relationship Between `downgrade` and `upgrade`**

1. **`Rc::downgrade`:**
    
    - Creates a weak reference to the same data without increasing the strong reference count.
    - Weak references do not own the data.
2. **`Weak::upgrade`:**
    
    - Attempts to convert a weak reference back to a strong one.
    - Fails (`None`) if the original strong reference has been dropped.