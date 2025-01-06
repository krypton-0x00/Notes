```rust
use std::rc::{Rc, Weak};
use std::cell::RefCell;

#[derive(Debug)]
struct TreeNode {
    value: i32,
    parent: RefCell<Weak<TreeNode>>,
    children: RefCell<Vec<Rc<TreeNode>>>,
}

impl TreeNode {
    // Constructor for a new node
    fn new(value: i32) -> Rc<TreeNode> {
        Rc::new(TreeNode {
            value,
            parent: RefCell::new(Weak::new()),
            children: RefCell::new(vec![]),
        })
    }

    // Add a child to the current node
    fn add_child(parent: &Rc<TreeNode>, child: Rc<TreeNode>) {
        // Set the child's parent to the current node
        *child.parent.borrow_mut() = Rc::downgrade(parent);
        // Add the child to the parent's children vector
        parent.children.borrow_mut().push(child);
    }
}

fn main() {
    // Create the root of the tree
    let root = TreeNode::new(1);

    // Create children
    let child1 = TreeNode::new(2);
    let child2 = TreeNode::new(3);

    // Add children to the root
    TreeNode::add_child(&root, Rc::clone(&child1));
    TreeNode::add_child(&root, Rc::clone(&child2));

    // Create a grandchild
    let grandchild = TreeNode::new(4);

    // Add grandchild to child1
    TreeNode::add_child(&child1, Rc::clone(&grandchild));

    // Visualize the tree
    println!("Root: {:?}", root);
    println!("Child1: {:?}", child1);
    println!("Child2: {:?}", child2);
    println!("Grandchild: {:?}", grandchild);

    // Check parent of grandchild
    if let Some(parent) = grandchild.parent.borrow().upgrade() {
        println!("Grandchild's parent: {:?}", parent.value);
    } else {
        println!("Grandchild has no parent");
    }
}

```