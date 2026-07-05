# Inorder

## Problem Statement
- traverse a binary tree to visit nodes in the left-root-right order.

## Approach / Intuition
- recursively visit the left subtree, then process the current root, and finally recursively visit the right subtree. In a [[Binary Search Tree]], this yields elements in sorted order.

## Time & Space Complexity
- **[[time Complexity]]:** O(N) where N is the number of nodes.
- **[[space Complexity]]:** O(H) where H is the height of the tree (for the recursion stack).

## Sample Code
```cpp
void inorder(TreeNode* root, vector<int>& res) {
    if (root) {
        inorder(root->left, res);
        res.push_back(root->val);
        inorder(root->right, res);
    }
}
```

## New Keywords / STL Used
- `std::vector::push_back`

## Edge Cases
- empty tree
- skewed tree (causing max recursion depth)

NEXT: [[Index]]
