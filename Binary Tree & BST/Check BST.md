# Check BST

## Problem Statement
- given the root of a binary tree, determine if it is a valid binary search tree (BST).

## Approach / Intuition
- a valid [[Binary Search Tree]] requires that all nodes in a node's left subtree have values strictly less than the node's value, and all nodes in its right subtree have values strictly greater. We can validate this by passing a min-max range (initially $[-\infty, \infty]$) down the recursive calls, ensuring each node strictly falls within its allowed range. Alternatively, we can verify that an [[Inorder Traversal]] produces a strictly increasing sequence.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$ since we visit each node once.
- **[[space Complexity]]:** $O(H)$ for the recursion stack.

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
- `lLONG_MIN`, `LLONG_MAX`

## Edge Cases
- tree with duplicate values
- single node tree
- values exactly equal to `INT_MAX` or `INT_MIN`

NEXT: [[Index]]
