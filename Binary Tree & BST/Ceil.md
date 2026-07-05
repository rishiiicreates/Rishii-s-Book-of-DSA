# Ceil

## Problem Statement
- given a BST and an integer key, find the ceil value of the key in the BST. The ceil value is the smallest node value greater than or equal to the key.

## Approach / Intuition
- similar to the floor problem, we traverse the [[Binary Search Tree]]. If the current node's value is equal to the key, it is our ceil. If the node's value is less than the key, the ceil must lie in the right subtree. If the node's value is greater than the key, this node is a valid candidate for the ceil. We record it and then move to the left subtree to see if we can find a smaller candidate that is still $\geq$ the key.

## Time & Space Complexity
- **[[time Complexity]]:** $O(H)$ where $H$ is the tree height.
- **[[space Complexity]]:** $O(1)$

## Sample Code
```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int ceilBST(TreeNode* root, int key) {
    int ceil = -1;
    while (root) {
        if (root->val == key) {
            return root->val;
        }
        
        if (key < root->val) {
            ceil = root->val;
            root = root->left;
        } else {
            root = root->right;
        }
    }
    return ceil;
}
```

## New Keywords / STL Used
- none specific

## Edge Cases
- key is greater than all nodes in the BST
- key is exactly present in the BST
- bST is empty

NEXT: [[Index]]
