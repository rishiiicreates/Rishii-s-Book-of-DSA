---
type: concept
tags: [binary_tree, bst, cpp, data-structure]
date: 2026-06-30
---
# Binary Tree

## Problem Statement
Represent a hierarchical structure where each element has at most two children.

## Approach / Intuition
Use a `struct` or `class` to define a node containing a value and two pointers (left and right). This is the foundation for more complex structures like a [[Binary Search Tree]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(1) for node creation.
- **[[Space Complexity]]:** O(N) where N is the number of nodes stored.

## Sample Code
```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

## New Keywords / STL Used
- `nullptr`, `struct`

## Edge Cases
- Empty tree (root is `nullptr`)
- Tree with a single node
