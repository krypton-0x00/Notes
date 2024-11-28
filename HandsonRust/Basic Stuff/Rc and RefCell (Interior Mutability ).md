```rust
use std::{cell::RefCell, rc::Rc};

fn interor_mutability() {
    let number = 0;
    let wrapped_number = Rc::new(RefCell::new(number));
    dbg!(wrapped_number.clone()); //value:0
    add_one(&wrapped_number);
    dbg!(wrapped_number); //value:1
}

fn add_one(wrapped_number: &Rc<RefCell<u32>>) {
    //we took the immutable ref in the param
    //but here we are mutating it compiler is yalling at us.
    let mut borrowed_number = wrapped_number.borrow_mut();
    *borrowed_number += 1;
}

fn main() {
    interor_mutability();
}
```