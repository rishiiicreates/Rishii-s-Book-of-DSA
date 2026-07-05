# Maximum

## Problem Statement
- find the node with the maximum value in a Binary Search Tree (BST).

## Approach / Intuition
- in a [[Binary Search Tree]], the right child of any node always contains a value greater than the node itself. To find the maximum value, we traverse to the rightmost leaf node starting from the root.

## Time & Space Complexity
- **[[time Complexity]]:** $O(H)$ where $H$ is the height of the tree.
- **[[space Complexity]]:** $O(1)$

## Sample Code
```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int maxValue(TreeNode* root) {
    if (!root) return -1;
    
    TreeNode* current = root;
    while (current->right != nullptr) {
        current = current->right;
    }
    return current->val;
}
```

## New Keywords / STL Used
- none specific

## Edge Cases
- tree is empty
- tree only has left children (skewed left)

NEXT: [[Index]]
