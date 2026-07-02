---
type: concept
tags: [binary_tree, bst, cpp, tree-view]
date: 2026-06-30
---
# Left View of Binary Tree

## Problem Statement
Given a binary tree, print or return the nodes visible when the tree is viewed from the left side.

## Approach / Intuition
Use a [[Depth-First Search]] (DFS) approach, maintaining the current depth. For each new depth level encountered, the first node visited will be the one visible from the left, provided we traverse the left child before the right child. Alternatively, [[Breadth-First Search]] (BFS) can be used, capturing the first node of each level queue.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(H)$ for DFS recursion stack, where $H$ is tree height

## Sample Code
```cpp
#include <vector>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

void solve(TreeNode* root, int level, vector<int>& res) {
    if (!root) return;
    if (res.size() == level) res.push_back(root->val);
    solve(root->left, level + 1, res);
    solve(root->right, level + 1, res);
}

vector<int> leftView(TreeNode *root) {
    vector<int> res;
    solve(root, 0, res);
    return res;
}
```

## New Keywords / STL Used
DFS traversal

## Edge Cases
Empty tree, completely skewed tree (left or right).
