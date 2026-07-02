---
type: concept
tags: [binary_tree, bst, cpp, recursion]
date: 2026-06-30
---
# Height

## Problem Statement
Find the maximum depth or height of a binary tree, defined as the number of nodes along the longest path from the root down to the farthest leaf.

## Approach / Intuition
Use a [[Depth First Search]] approach. The height of a tree is 1 plus the maximum of the heights of its left and right subtrees.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the number of nodes.
- **[[Space Complexity]]:** O(H) for recursion stack.

## Sample Code
```cpp
int treeHeight(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(treeHeight(root->left), treeHeight(root->right));
}
```

## New Keywords / STL Used
- `std::max`

## Edge Cases
- Empty tree (height is 0)
- Tree with just one root node (height is 1)
