# Preorder

## Problem Statement
- traverse a binary tree to visit nodes in the root-left-right order.

## Approach / Intuition
- process the current root first, then recursively visit the left subtree, followed by the right subtree. This is useful for creating a copy of the tree or [[Prefix Notation]] expressions.

## Time & Space Complexity
- **[[time Complexity]]:** O(N)
- **[[space Complexity]]:** O(H) recursion stack height.

## Sample Code
```cpp
void preorder(TreeNode* root, vector<int>& res) {
    if (root) {
        res.push_back(root->val);
        preorder(root->left, res);
        preorder(root->right, res);
    }
}
```

## New Keywords / STL Used
- none

## Edge Cases
- empty tree
- skewed tree

NEXT: [[Index]]
