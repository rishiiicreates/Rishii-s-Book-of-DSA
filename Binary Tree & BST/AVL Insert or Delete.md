# AVL Insert or Delete

## Problem Statement
- implement the insertion and deletion operations in an AVL tree, ensuring that the tree remains height-balanced after each operation.

## Approach / Intuition
- an [[AVL Tree]] is a self-balancing binary search tree where the difference between heights of left and right subtrees cannot be more than one for all nodes. When we insert or delete, we first perform the standard BST operation. Then, we update the heights and calculate the balance factor. If the node becomes unbalanced, we perform one of four [[Tree Rotations]] (Left-Left, Right-Right, Left-Right, or Right-Left) to restore balance.

## Time & Space Complexity
- **[[time Complexity]]:** $O(\log N)$ for both insert and delete.
- **[[space Complexity]]:** $O(\log N)$ for the recursion stack.

## Sample Code
```cpp
#include <algorithm>

struct Node {
    int key, height;
    Node *left, *right;
    Node(int k) : key(k), height(1), left(nullptr), right(nullptr) {}
};

int height(Node* N) {
    if (!N) return 0;
    return N->height;
}

int getBalance(Node* N) {
    if (!N) return 0;
    return height(N->left) - height(N->right);
}

Node* rightRotate(Node* y) {
    Node* x = y->left;
    Node* T2 = x->right;
    x->right = y;
    y->left = T2;
    y->height = std::max(height(y->left), height(y->right)) + 1;
    x->height = std::max(height(x->left), height(x->right)) + 1;
    return x;
}

Node* leftRotate(Node* x) {
    Node* y = x->right;
    Node* T2 = y->left;
    y->left = x;
    x->right = T2;
    x->height = std::max(height(x->left), height(x->right)) + 1;
    y->height = std::max(height(y->left), height(y->right)) + 1;
    return y;
}

Node* insert(Node* node, int key) {
    if (!node) return new Node(key);
    
    if (key < node->key)
        node->left = insert(node->left, key);
    else if (key > node->key)
        node->right = insert(node->right, key);
    else
        return node;
        
    node->height = 1 + std::max(height(node->left), height(node->right));
    int balance = getBalance(node);
    
    if (balance > 1 && key < node->left->key) return rightRotate(node);
    if (balance < -1 && key > node->right->key) return leftRotate(node);
    if (balance > 1 && key > node->left->key) {
        node->left = leftRotate(node->left);
        return rightRotate(node);
    }
    if (balance < -1 && key < node->right->key) {
        node->right = rightRotate(node->right);
        return leftRotate(node);
    }
    
    return node;
}
```

## New Keywords / STL Used
- `std::max`

## Edge Cases
- inserting sorted data (would cause worst-case $O(N)$ in standard BST, but $O(\log N)$ in AVL)
- duplicate keys (often ignored or stored with counts)

NEXT: [[Index]]
