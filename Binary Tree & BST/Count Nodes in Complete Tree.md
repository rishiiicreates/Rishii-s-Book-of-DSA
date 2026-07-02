---
type: concept
tags: [binary_tree, bst, cpp, complete-tree]
date: 2026-06-30
---
# Count Complete Tree Nodes

## Problem Statement
Given the root of a complete binary tree, return the number of the nodes in the tree efficiently (faster than $O(N)$).

## Approach / Intuition
In a complete binary tree, if the depth of the leftmost path equals the depth of the rightmost path, it's a perfect binary tree, and its node count is $2^h - 1$. If they are not equal, recursively count the nodes in the left and right subtrees. This approach uses [[Depth-First Search]] combined with structural properties to prune large parts of the tree.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(\log^2 N)$
- **[[Space Complexity]]:** $O(\log N)$ for recursion stack

## Sample Code
```cpp
#include <cmath>

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

int findLeftHeight(TreeNode* node) {
    int height = 0;
    while (node) {
        height++;
        node = node->left;
    }
    return height;
}

int findRightHeight(TreeNode* node) {
    int height = 0;
    while (node) {
        height++;
        node = node->right;
    }
    return height;
}

int countNodes(TreeNode* root) {
    if (root == NULL) return 0;
    
    int lh = findLeftHeight(root);
    int rh = findRightHeight(root);
    
    if (lh == rh) return (1 << lh) - 1;
    
    return 1 + countNodes(root->left) + countNodes(root->right);
}
```

## New Keywords / STL Used
Bitwise shift for power of 2

## Edge Cases
Empty tree, tree with just one node, last level with just one node.
