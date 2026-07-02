---
type: concept
tags: [binary_tree, bst, cpp, insert]
date: 2026-06-30
---
# Insert

## Problem Statement
Given the root of a Binary Search Tree (BST) and a value to insert, insert the value into the BST and return the root. It is guaranteed that the new value does not exist in the original BST.

## Approach / Intuition
To insert a new node while maintaining the [[Binary Search Tree]] property, we traverse the tree from the root. At each step, if the new value is smaller than the current node's value, we go left; if greater, we go right. We continue this until we reach a null pointer, which is the correct position to insert the new node. We can perform this recursively or iteratively.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(H)$ where $H$ is the height of the tree.
- **[[Space Complexity]]:** $O(1)$ for iterative approach.

## Sample Code
```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

TreeNode* insertIntoBST(TreeNode* root, int val) {
    if (!root) return new TreeNode(val);
    
    TreeNode* curr = root;
    while (true) {
        if (val < curr->val) {
            if (curr->left) curr = curr->left;
            else {
                curr->left = new TreeNode(val);
                break;
            }
        } else {
            if (curr->right) curr = curr->right;
            else {
                curr->right = new TreeNode(val);
                break;
            }
        }
    }
    
    return root;
}
```

## New Keywords / STL Used
`new`

## Edge Cases
- Inserting into an empty tree
- Value becomes the new minimum or maximum leaf
