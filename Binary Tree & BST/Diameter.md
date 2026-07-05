# Diameter of Binary Tree

## Problem Statement
- compute the length of the diameter of a binary tree. The diameter is the length of the longest path between any two nodes in a tree, which may or may not pass through the root.

## Approach / Intuition
- the longest path passing through a node is the sum of the maximum heights of its left and right subtrees. Compute the height using [[Depth-First Search]], and at each node, update a global maximum diameter with `left_height + right_height`.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$
- **[[space Complexity]]:** $O(H)$ for recursion stack

## Sample Code
```cpp
#include <algorithm>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

int height(TreeNode* node, int& diameter) {
    if (!node) return 0;
    
    int lh = height(node->left, diameter);
    int rh = height(node->right, diameter);
    
    diameter = max(diameter, lh + rh);
    
    return 1 + max(lh, rh);
}

int diameterOfBinaryTree(TreeNode* root) {
    int diameter = 0;
    height(root, diameter);
    return diameter;
}
```

## New Keywords / STL Used
- pass by reference

## Edge Cases
- empty tree, tree with one node, heavily unbalanced tree.

NEXT: [[Index]]
