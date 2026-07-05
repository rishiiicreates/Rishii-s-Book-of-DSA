# Postorder

## Problem Statement
- traverse a binary tree to visit nodes in the left-right-root order.

## Approach / Intuition
- recursively visit the left subtree, then the right subtree, and finally process the current root. This is typically used when you need to delete a tree or evaluate [[Postfix Notation]] expressions.

## Time & Space Complexity
- **[[time Complexity]]:** O(N)
- **[[space Complexity]]:** O(H) for recursion stack.

## Sample Code
```cpp
void postorder(TreeNode* root, vector<int>& res) {
    if (root) {
        postorder(root->left, res);
        postorder(root->right, res);
        res.push_back(root->val);
    }
}
```

## New Keywords / STL Used
- none

## Edge Cases
- empty tree
- skewed tree

NEXT: [[Index]]
