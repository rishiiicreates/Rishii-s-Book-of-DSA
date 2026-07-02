---
type: concept
tags: [binary_tree, bst, cpp, validation]
date: 2026-06-30
---
# Check BST

## Problem Statement
Given the root of a binary tree, determine if it is a valid binary search tree (BST).

## Approach / Intuition
A valid [[Binary Search Tree]] requires that all nodes in a node's left subtree have values strictly less than the node's value, and all nodes in its right subtree have values strictly greater. We can validate this by passing a min-max range (initially $[-\infty, \infty]$) down the recursive calls, ensuring each node strictly falls within its allowed range. Alternatively, we can verify that an [[Inorder Traversal]] produces a strictly increasing sequence.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$ since we visit each node once.
- **[[Space Complexity]]:** $O(H)$ for the recursion stack.

## Sample Code
```cpp
#include <climits>

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

bool isValidBSTUtil(TreeNode* node, long long minVal, long long maxVal) {
    if (!node) return true;
    
    if (node->val <= minVal || node->val >= maxVal) {
        return false;
    }
    
    return isValidBSTUtil(node->left, minVal, node->val) && 
           isValidBSTUtil(node->right, node->val, maxVal);
}

bool isValidBST(TreeNode* root) {
    return isValidBSTUtil(root, LLONG_MIN, LLONG_MAX);
}
```

## New Keywords / STL Used
`LLONG_MIN`, `LLONG_MAX`

## Edge Cases
- Tree with duplicate values
- Single node tree
- Values exactly equal to `INT_MAX` or `INT_MIN`
