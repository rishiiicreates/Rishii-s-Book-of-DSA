---
type: concept
tags: [binary_tree, bst, cpp, traversal]
date: 2026-06-30
---
# Preorder

## Problem Statement
Traverse a binary tree to visit nodes in the root-left-right order.

## Approach / Intuition
Process the current root first, then recursively visit the left subtree, followed by the right subtree. This is useful for creating a copy of the tree or [[Prefix Notation]] expressions.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N)
- **[[Space Complexity]]:** O(H) recursion stack height.

## Sample Code
```cpp
void preorder(TreeNode* root, vector<int>& res) {
    if (root) {
        res.push_back(root->val);
        preorder(root->left, res);
        preorder(root->right, res);
    }
}
```

## New Keywords / STL Used
- None

## Edge Cases
- Empty tree
- Skewed tree
