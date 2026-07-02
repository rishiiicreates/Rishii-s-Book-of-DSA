---
type: concept
tags: [binary_tree, bst, cpp, delete]
date: 2026-06-30
---
# Delete

## Problem Statement
Given a root node reference of a BST and a key, delete the node with the given key in the BST and return the root node reference.

## Approach / Intuition
When deleting a node in a [[Binary Search Tree]], there are three cases. If the node is a leaf, simply remove it. If it has one child, replace the node with its child. If it has two children, find its [[Inorder Successor]] (minimum value in the right subtree) or Inorder Predecessor, replace the node's value with the successor's value, and then recursively delete the successor.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(H)$ where $H$ is the height of the tree.
- **[[Space Complexity]]:** $O(H)$ for recursive call stack.

## Sample Code
```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

TreeNode* deleteNode(TreeNode* root, int key) {
    if (!root) return root;

    if (key < root->val) {
        root->left = deleteNode(root->left, key);
    } else if (key > root->val) {
        root->right = deleteNode(root->right, key);
    } else {
        if (!root->left) {
            TreeNode* temp = root->right;
            delete root;
            return temp;
        } else if (!root->right) {
            TreeNode* temp = root->left;
            delete root;
            return temp;
        }

        TreeNode* temp = root->right;
        while (temp && temp->left) {
            temp = temp->left;
        }
        
        root->val = temp->val;
        root->right = deleteNode(root->right, temp->val);
    }
    return root;
}
```

## New Keywords / STL Used
`delete`

## Edge Cases
- Deleting a leaf node
- Deleting a node with one child
- Deleting the root node
- Node to delete is not present
