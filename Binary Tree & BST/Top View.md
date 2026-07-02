---
type: concept
tags: [binary_tree, bst, cpp, tree-view]
date: 2026-06-30
---
# Top View of Binary Tree

## Problem Statement
Given a binary tree, return the nodes visible when viewed from the top, ordered from left to right.

## Approach / Intuition
Assign a horizontal distance (HD) to each node, where the root is at HD 0, the left child is at HD - 1, and the right child is at HD + 1. Perform a [[Breadth-First Search]] (Level Order Traversal) to ensure we capture the uppermost nodes first. Use a map to store the first node encountered at each HD.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N \log N)$ due to map insertion, $O(N)$ if hash map is used
- **[[Space Complexity]]:** $O(N)$ for queue and map

## Sample Code
```cpp
#include <vector>
#include <map>
#include <queue>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

vector<int> topView(TreeNode *root) {
    vector<int> ans;
    if (!root) return ans;
    
    map<int, int> mpp;
    queue<pair<TreeNode*, int>> q;
    q.push({root, 0});
    
    while (!q.empty()) {
        auto it = q.front();
        q.pop();
        TreeNode* node = it.first;
        int line = it.second;
        
        if (mpp.find(line) == mpp.end()) {
            mpp[line] = node->val;
        }
        
        if (node->left) q.push({node->left, line - 1});
        if (node->right) q.push({node->right, line + 1});
    }
    
    for (auto it : mpp) {
        ans.push_back(it.second);
    }
    return ans;
}
```

## New Keywords / STL Used
`map`, `queue`, `pair`

## Edge Cases
Empty tree, completely overlapped nodes in vertical projection.
