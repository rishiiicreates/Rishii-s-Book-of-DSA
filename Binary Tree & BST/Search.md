---
type: concept
tags: [binary_tree, bst, cpp, search]
date: 2026-06-30
---
# Search

## Problem Statement
Given the root of a Binary Search Tree (BST) and a target value, return the node containing the target value. If such a node does not exist, return null.

## Approach / Intuition
We use the properties of a [[Binary Search Tree]]. Starting from the root, if the target is equal to the current node's value, we've found it. If the target is less than the current node's value, we recursively or iteratively search the left subtree. If the target is greater, we search the right subtree. This is analogous to a [[Binary Search]] algorithm.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(H)$ where $H$ is the height of the tree (average $O(\log N)$, worst-case $O(N)$ for skewed trees).
- **[[Space Complexity]]:** $O(1)$ for iterative approach.

## Sample Code
```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

TreeNode* searchBST(TreeNode* root, int val) {
    while (root != nullptr && root->val != val) {
        if (val < root->val) {
            root = root->left;
        } else {
            root = root->right;
        }
    }
    return root;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
- Empty tree
- Target not present in the tree
- Target is the root node
