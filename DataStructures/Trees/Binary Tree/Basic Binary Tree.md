Key terminologies:

- **Root**: The topmost node in the tree.
- **Node**: An element in the tree.
- **Edge**: A connection between two nodes.
- **Child**: A node that is directly connected to another node when moving away from the root.
- **Parent**: The converse of a child.
- **Leaf**: A node that has no children.
- **Subtree**: A tree consisting of a node and its descendants.
- **Height**: The longest path from the root to a leaf.

#### (a) **Binary Tree**

A **binary tree** is a tree where each node has at most two children, referred to as the **left child** and the **right child**. This is one of the most commonly used tree structures.

#### (b) **Binary Search Tree (BST)**

A **Binary Search Tree** is a binary tree with the following properties:

- The left subtree contains only nodes with values **less than** the node’s value.
- The right subtree contains only nodes with values **greater than** the node’s value.
- Both left and right subtrees must also be binary search trees.

![[Pasted image 20241011145912.png]]


```cpp
struct Node {
    int data;
    Node* left;
    Node* right;
    
    Node(int data) {
        this->data = data;
        this->left = this->right = nullptr;
    }
};
Node* createNode(int data) {
    Node* newNode = new Node(12);
    return newNode;
}


int main()
{
    Node* root = createNode(1);
    root->left = createNode(2);
    root->right = createNode(3); 
    root->left->left = createNode(4);
}

```


# Binary Tree in action


```cpp
struct Node {
    int data;
    Node* left;
    Node* right;

    // Constructor
    Node(int value) {
        data = value;
        left = nullptr;
        right = nullptr;
    }
};

// Insertion into a Binary Search Tree (BST)
Node* insert(Node* root, int value) {
    if (root == nullptr) {
        return new Node(value); 
    }

    // Recursively insert the value
    if (value < root->data) {
        root->left = insert(root->left, value);
    } else {
        root->right = insert(root->right, value);
    }

    return root;
}

```
### **Tree Traversal Methods**

Traversal is the process of visiting all the nodes in the tree. There are different ways to traverse a tree:

- **Inorder (Left, Root, Right)**: Used in binary search trees to get nodes in non-decreasing order.
- **Preorder (Root, Left, Right)**: Useful for creating a copy of the tree.
- **Postorder (Left, Right, Root)**: Useful for deleting a tree.
- **Level Order (Breadth-First Search)**: Visit nodes level by level, from top to bottom.
# Inorder Travrsal
**Inorder Traversal** is a tree traversal technique used to visit every node in a **binary tree** in a specific order. In an **inorder traversal**, the nodes are visited in the following order:

1. **Left Subtree**
2. **Root (Current Node)**
3. **Right Subtree**

### Key Characteristics:

- **In a Binary Search Tree (BST)**, an inorder traversal visits the nodes in **sorted (non-decreasing) order**. This is one of the most common applications of inorder traversal.
- It's a **depth-first traversal** technique, meaning it explores as far down a subtree as possible before moving to the next subtree.

### Inorder Traversal Algorithm

The steps for inorder traversal are:

1. Traverse the left subtree by recursively applying the inorder traversal.
2. Visit the current node (root).
3. Traverse the right subtree by recursively applying the inorder traversal.

### Example of Inorder Traversal

Consider the following binary tree:

```markdown
        50
       /  \
     30    70
    /  \  /  \
   20  40 60  80

```
output-> 20 30 40 50 60 70 80

Start by going to the leftmost node (`20`), then visit its parent (`30`), and its right child (`40`), and so on.
```cpp
#include <iostream>

struct Node {
    int data;
    Node* left;
    Node* right;

    Node(int value) {
        data = value;
        left = nullptr;
        right = nullptr;
    }
};

void inorderTraversal(Node* root) {
    if (root == nullptr) {
        return;  // Base case: If the node is empty, return
    }

    inorderTraversal(root->left);   // Visit left subtree
    std::cout << root->data << " "; // Visit current node
    inorderTraversal(root->right);  // Visit right subtree
}

int main() {
    // Creating a binary tree
    Node* root = new Node(50);
    root->left = new Node(30);
    root->right = new Node(70);
    root->left->left = new Node(20);
    root->left->right = new Node(40);
    root->right->left = new Node(60);
    root->right->right = new Node(80);

    std::cout << "Inorder Traversal: ";
    inorderTraversal(root);  // Should print nodes in ascending order
    std::cout << std::endl;

    return 0;
}

```