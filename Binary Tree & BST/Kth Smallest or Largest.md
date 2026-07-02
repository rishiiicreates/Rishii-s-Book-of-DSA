---
type: concept
tags: [binary_tree, bst, cpp, inorder, kth]
date: 2026-06-30
---
# Kth Smallest or Largest

## Problem Statement
Given the root of a BST and an integer $k$, find the $k^{th}$ smallest (or largest) element in the tree.

## Approach / Intuition
An [[Inorder Traversal]] of a BST visits the nodes in sorted ascending order. To find the $k^{th}$ smallest element, we can perform an inorder traversal and keep a counter. When the counter reaches $k$, we have found our target. Conversely, a Reverse Inorder Traversal (Right -> Root -> Left) visits nodes in descending order, which perfectly solves the $k^{th}$ largest element problem.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(H + K)$ for traversal up to the $k^{th}$ node.
- **[[Space Complexity]]:** $O(H)$ for the recursive call stack.

## Sample Code
```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

void inorder(TreeNode* root, int& k, int& ans) {
    if (!root || k == 0) return;
    
    inorder(root->left, k, ans);
    
    k--;
    if (k == 0) {
        ans = root->val;
        return;
    }
    
    inorder(root->right, k, ans);
}

int kthSmallest(TreeNode* root, int k) {
    int ans = -1;
    inorder(root, k, ans);
    return ans;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
- $k$ is $1$ (minimum or maximum)
- $k$ is the total number of nodes
- $k$ is larger than the number of nodes
