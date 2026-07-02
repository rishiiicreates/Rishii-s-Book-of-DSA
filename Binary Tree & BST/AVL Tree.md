---
type: concept
tags: [binary_tree, bst, cpp, balanced-tree]
date: 2026-06-30
---
# AVL Tree

## Problem Statement
Maintain a balanced binary search tree to guarantee O(log N) operations even in the worst case.

## Approach / Intuition
An [[AVL Tree]] is a self-balancing BST where the difference between heights of left and right subtrees cannot be more than one. If violated during insertion or deletion, we use [[Tree Rotations]] to restore balance.

## Time & Space Complexity
- **[[Time Complexity]]:** O(log N) for search, insert, and delete.
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
struct AVLNode {
    int val, height;
    AVLNode *left, *right;
    AVLNode(int x) : val(x), height(1), left(nullptr), right(nullptr) {}
};

int height(AVLNode* N) {
    return N ? N->height : 0;
}

AVLNode* rightRotate(AVLNode* y) {
    AVLNode* x = y->left;
    AVLNode* T2 = x->right;
    x->right = y;
    y->left = T2;
    y->height = max(height(y->left), height(y->right)) + 1;
    x->height = max(height(x->left), height(x->right)) + 1;
    return x;
}
```

## New Keywords / STL Used
- `std::max`

## Edge Cases
- Skewed inputs resulting in multiple consecutive rotations
- Empty tree
