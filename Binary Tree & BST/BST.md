# BST

## Problem Statement
- implement a tree structure that allows for fast search, insertion, and deletion of items.

## Approach / Intuition
- in a [[Binary Search Tree]], the left child contains values strictly less than the parent node, and the right child contains values strictly greater. This property enables [[Binary Search]]-like navigation.

## Time & Space Complexity
- **[[time Complexity]]:** O(log N) average, O(N) worst case (skewed tree).
- **[[space Complexity]]:** O(N)

## Sample Code
```cpp
TreeNode* insertBST(TreeNode* root, int val) {
    if (!root) return new TreeNode(val);
    if (val < root->val) {
        root->left = insertBST(root->left, val);
    } else if (val > root->val) {
        root->right = insertBST(root->right, val);
    }
    return root;
}
```

## New Keywords / STL Used
- none

## Edge Cases
- inserting duplicates (usually ignored or tracked via count)
- highly skewed insertions (e.g., sorted order)

NEXT: [[Index]]
