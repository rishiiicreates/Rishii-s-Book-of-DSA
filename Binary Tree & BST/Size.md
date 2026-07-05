# Size

## Problem Statement
- calculate the total number of nodes in a binary tree.

## Approach / Intuition
- use recursion. The size of the tree is 1 (for the current root) plus the size of its left subtree plus the size of its right subtree. This is a basic application of [[Tree Traversal]].

## Time & Space Complexity
- **[[time Complexity]]:** O(N)
- **[[space Complexity]]:** O(H) for the recursion stack depth.

## Sample Code
```cpp
int treeSize(TreeNode* root) {
    if (!root) return 0;
    return 1 + treeSize(root->left) + treeSize(root->right);
}
```

## New Keywords / STL Used
- none

## Edge Cases
- empty tree
- tree with a single node

NEXT: [[Index]]
