# Binary Tree

## Problem Statement
- represent a hierarchical structure where each element has at most two children.

## Approach / Intuition
- use a `struct` or `class` to define a node containing a value and two pointers (left and right). This is the foundation for more complex structures like a [[Binary Search Tree]].

## Time & Space Complexity
- **[[time Complexity]]:** O(1) for node creation.
- **[[space Complexity]]:** O(N) where N is the number of nodes stored.

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
- empty tree (root is `nullptr`)
- tree with a single node

NEXT: [[Index]]
