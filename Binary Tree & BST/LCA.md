---
type: concept
tags: [binary_tree, bst, cpp, lca]
date: 2026-06-30
---
# Lowest Common Ancestor (LCA) in a Binary Tree

## Problem Statement
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

## Approach / Intuition
Traverse the tree using [[Depth-First Search]]. If the current node is one of the target nodes, return it. Recursively search the left and right subtrees. If both left and right recursive calls return a non-null node, the current node is the LCA. If only one returns a non-null node, propagate that node upwards.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(H)$ for recursion stack

## Sample Code
```cpp
#include <cstddef>

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (root == NULL || root == p || root == q) {
        return root;
    }
    
    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    TreeNode* right = lowestCommonAncestor(root->right, p, q);
    
    if (left == NULL) {
        return right;
    } else if (right == NULL) {
        return left;
    } else {
        return root;
    }
}
```

## New Keywords / STL Used
Recursive object returning

## Edge Cases
One node is the ancestor of the other, targets are not in the tree (if not guaranteed).
