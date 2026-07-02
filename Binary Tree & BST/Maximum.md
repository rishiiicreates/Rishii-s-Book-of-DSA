---
type: concept
tags: [binary_tree, bst, cpp, maximum]
date: 2026-06-30
---
# Maximum

## Problem Statement
Find the node with the maximum value in a Binary Search Tree (BST).

## Approach / Intuition
In a [[Binary Search Tree]], the right child of any node always contains a value greater than the node itself. To find the maximum value, we traverse to the rightmost leaf node starting from the root.

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

int maxValue(TreeNode* root) {
    if (!root) return -1;
    
    TreeNode* current = root;
    while (current->right != nullptr) {
        current = current->right;
    }
    return current->val;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
- Tree is empty
- Tree only has left children (skewed left)
