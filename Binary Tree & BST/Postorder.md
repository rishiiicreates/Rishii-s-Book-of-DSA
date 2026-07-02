---
type: concept
tags: [binary_tree, bst, cpp, traversal]
date: 2026-06-30
---
# Postorder

## Problem Statement
Traverse a binary tree to visit nodes in the left-right-root order.

## Approach / Intuition
Recursively visit the left subtree, then the right subtree, and finally process the current root. This is typically used when you need to delete a tree or evaluate [[Postfix Notation]] expressions.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N)
- **[[Space Complexity]]:** O(H) for recursion stack.

## Sample Code
```cpp
void postorder(TreeNode* root, vector<int>& res) {
    if (root) {
        postorder(root->left, res);
        postorder(root->right, res);
        res.push_back(root->val);
    }
}
```

## New Keywords / STL Used
- None

## Edge Cases
- Empty tree
- Skewed tree
