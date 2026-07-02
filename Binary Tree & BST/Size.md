---
type: concept
tags: [binary_tree, bst, cpp, recursion]
date: 2026-06-30
---
# Size

## Problem Statement
Calculate the total number of nodes in a binary tree.

## Approach / Intuition
Use recursion. The size of the tree is 1 (for the current root) plus the size of its left subtree plus the size of its right subtree. This is a basic application of [[Tree Traversal]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(N)
- **[[Space Complexity]]:** O(H) for the recursion stack depth.

## Sample Code
```cpp
int treeSize(TreeNode* root) {
    if (!root) return 0;
    return 1 + treeSize(root->left) + treeSize(root->right);
}
```

## New Keywords / STL Used
- None

## Edge Cases
- Empty tree
- Tree with a single node
