# Minimum

## Problem Statement
- find the node with the minimum value in a Binary Search Tree (BST).

## Approach / Intuition
- because of the properties of a [[Binary Search Tree]], the left child of any node always contains a smaller value than the node itself. Therefore, to find the minimum value, we simply need to traverse to the leftmost leaf node starting from the root.

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

int minValue(TreeNode* root) {
    if (!root) return -1;
    
    TreeNode* current = root;
    while (current->left != nullptr) {
        current = current->left;
    }
    return current->val;
}
```

## New Keywords / STL Used
- none specific

## Edge Cases
- tree is empty
- tree only has right children (skewed right)

NEXT: [[Index]]
