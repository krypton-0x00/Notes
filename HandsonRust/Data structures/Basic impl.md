Implementing basic data structures in Rust is a great way to strengthen your understanding of both the language and foundational computer science concepts. Here's how to implement some commonly used data structures:

---

### **1. Stack**

A stack follows the **LIFO** (Last In, First Out) principle. We'll implement it using a `Vec`.

```rust
struct Stack<T> {
    items: Vec<T>,
}

impl<T> Stack<T> {
    fn new() -> Self {
        Stack { items: Vec::new() }
    }

    fn push(&mut self, item: T) {
        self.items.push(item);
    }

    fn pop(&mut self) -> Option<T> {
        self.items.pop()
    }

    fn peek(&self) -> Option<&T> {
        self.items.last()
    }

    fn is_empty(&self) -> bool {
        self.items.is_empty()
    }

    fn size(&self) -> usize {
        self.items.len()
    }
}

fn main() {
    let mut stack = Stack::new();
    stack.push(10);
    stack.push(20);
    println!("{:?}", stack.pop()); // Output: Some(20)
    println!("{:?}", stack.peek()); // Output: Some(10)
}
```

---

### **2. Queue**

A queue follows the **FIFO** (First In, First Out) principle. We'll use a `VecDeque` from Rust's standard library.

```rust
use std::collections::VecDeque;

struct Queue<T> {
    items: VecDeque<T>,
}

impl<T> Queue<T> {
    fn new() -> Self {
        Queue {
            items: VecDeque::new(),
        }
    }

    fn enqueue(&mut self, item: T) {
        self.items.push_back(item);
    }

    fn dequeue(&mut self) -> Option<T> {
        self.items.pop_front()
    }

    fn peek(&self) -> Option<&T> {
        self.items.front()
    }

    fn is_empty(&self) -> bool {
        self.items.is_empty()
    }

    fn size(&self) -> usize {
        self.items.len()
    }
}

fn main() {
    let mut queue = Queue::new();
    queue.enqueue(1);
    queue.enqueue(2);
    println!("{:?}", queue.dequeue()); // Output: Some(1)
    println!("{:?}", queue.peek());    // Output: Some(2)
}
```

---

### **3. Singly Linked List**

A simple implementation of a singly linked list.

```rust
use std::rc::Rc;
use std::cell::RefCell;

type Link<T> = Option<Rc<RefCell<Node<T>>>>;

struct Node<T> {
    value: T,
    next: Link<T>,
}

struct LinkedList<T> {
    head: Link<T>,
}

impl<T> LinkedList<T> {
    fn new() -> Self {
        LinkedList { head: None }
    }

    fn push(&mut self, value: T) {
        let new_node = Rc::new(RefCell::new(Node {
            value,
            next: self.head.take(),
        }));
        self.head = Some(new_node);
    }

    fn pop(&mut self) -> Option<T> {
        self.head.take().map(|node| {
            let node = Rc::try_unwrap(node).ok().unwrap().into_inner();
            self.head = node.next;
            node.value
        })
    }

    fn peek(&self) -> Option<T>
    where
        T: Clone,
    {
        self.head.as_ref().map(|node| node.borrow().value.clone())
    }
}

fn main() {
    let mut list = LinkedList::new();
    list.push(1);
    list.push(2);
    println!("{:?}", list.pop()); // Output: Some(2)
    println!("{:?}", list.peek()); // Output: Some(1)
}
```

---

### **4. Binary Tree**

Here’s a simple binary tree implementation.

```rust
struct TreeNode<T> {
    value: T,
    left: Option<Box<TreeNode<T>>>,
    right: Option<Box<TreeNode<T>>>,
}

impl<T> TreeNode<T> {
    fn new(value: T) -> Self {
        TreeNode {
            value,
            left: None,
            right: None,
        }
    }
}

struct BinaryTree<T> {
    root: Option<Box<TreeNode<T>>>,
}

impl<T> BinaryTree<T> {
    fn new() -> Self {
        BinaryTree { root: None }
    }

    fn insert(&mut self, value: T)
    where
        T: PartialOrd,
    {
        fn insert_node<T>(node: &mut Option<Box<TreeNode<T>>>, value: T)
        where
            T: PartialOrd,
        {
            match node {
                Some(ref mut n) => {
                    if value < n.value {
                        insert_node(&mut n.left, value);
                    } else {
                        insert_node(&mut n.right, value);
                    }
                }
                None => {
                    *node = Some(Box::new(TreeNode::new(value)));
                }
            }
        }

        insert_node(&mut self.root, value);
    }
}

fn main() {
    let mut tree = BinaryTree::new();
    tree.insert(10);
    tree.insert(5);
    tree.insert(15);
    println!("Binary tree root value: {:?}", tree.root.unwrap().value); // Output: 10
}
```

---

### **5. HashMap**

Rust's standard library provides an efficient `HashMap`. Here’s a simple wrapper to demonstrate basic usage:

```rust
use std::collections::HashMap;

struct MyHashMap<K, V> {
    map: HashMap<K, V>,
}

impl<K, V> MyHashMap<K, V>
where
    K: std::cmp::Eq + std::hash::Hash,
{
    fn new() -> Self {
        MyHashMap {
            map: HashMap::new(),
        }
    }

    fn insert(&mut self, key: K, value: V) {
        self.map.insert(key, value);
    }

    fn get(&self, key: &K) -> Option<&V> {
        self.map.get(key)
    }

    fn remove(&mut self, key: &K) -> Option<V> {
        self.map.remove(key)
    }
}

fn main() {
    let mut my_map = MyHashMap::new();
    my_map.insert("key1", 10);
    println!("{:?}", my_map.get(&"key1")); // Output: Some(10)
    my_map.remove(&"key1");
    println!("{:?}", my_map.get(&"key1")); // Output: None
}
```

---

These examples focus on **idiomatic Rust**, emphasizing safety and performance. Let me know if you'd like to expand any of these or explore more advanced data structures!