---
type: concept
tags: [binary_tree, bst, cpp, traversal]
date: 2026-06-30
---
# Inorder

## Problem Statement
Traverse a binary tree to visit nodes in the left-root-right order.

## Approach / Intuition
Recursively visit the left subtree, then process the current root, and finally recursively visit the right subtree. In a [[Binary Search Tree]], this yields elements in sorted order.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the number of nodes.
- **[[Space Complexity]]:** O(H) where H is the height of the tree (for the recursion stack).

## Sample Code
```cpp
void inorder(TreeNode* root, vector<int>& res) {
    if (root) {
        inorder(root->left, res);
        res.push_back(root->val);
        inorder(root->right, res);
    }
}
```

## New Keywords / STL Used
- `std::vector::push_back`

## Edge Cases
- Empty tree
- Skewed tree (causing max recursion depth)
