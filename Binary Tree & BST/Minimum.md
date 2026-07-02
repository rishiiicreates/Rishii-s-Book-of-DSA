---
type: concept
tags: [binary_tree, bst, cpp, minimum]
date: 2026-06-30
---
# Minimum

## Problem Statement
Find the node with the minimum value in a Binary Search Tree (BST).

## Approach / Intuition
Because of the properties of a [[Binary Search Tree]], the left child of any node always contains a smaller value than the node itself. Therefore, to find the minimum value, we simply need to traverse to the leftmost leaf node starting from the root.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(H)$ where $H$ is the height of the tree.
- **[[Space Complexity]]:** $O(1)$

## Sample Code
```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int minValue(TreeNode* root) {
    if (!root) return -1;
    
    TreeNode* current = root;
    while (current->left != nullptr) {
        current = current->left;
    }
    return current->val;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
- Tree is empty
- Tree only has right children (skewed right)
