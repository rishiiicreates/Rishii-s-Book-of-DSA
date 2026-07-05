# Predecessor and Successor

## Problem Statement
- find the inorder predecessor and successor of a given key in a [[Binary Search Tree]].

## Approach / Intuition
- traverse the tree from the root. To find the predecessor, move left if the key is smaller or equal, and move right while keeping track of the potential predecessor. To find the successor, move right if the key is greater or equal, and move left while tracking the potential successor. The inorder traversal of a BST gives elements in sorted order.

## Time & Space Complexity
- **[[time Complexity]]:** O(H) where H is the height of the tree.
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
void findPreSuc(TreeNode* root, TreeNode*& pre, TreeNode*& suc, int key) {
    TreeNode* curr = root;
    while (curr) {
        if (curr->val > key) {
            suc = curr;
            curr = curr->left;
        } else {
            curr = curr->right;
        }
    }
    
    curr = root;
    while (curr) {
        if (curr->val < key) {
            pre = curr;
            curr = curr->right;
        } else {
            curr = curr->left;
        }
    }
}
```

## New Keywords / STL Used
- reference to pointer `TreeNode*&`

## Edge Cases
- key not present in the tree.
- key is the maximum or minimum element.

NEXT: [[Index]]
