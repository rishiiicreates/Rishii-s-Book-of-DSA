---
type: concept
tags: [binary_tree, bst, cpp, tree-view]
date: 2026-06-30
---
# Vertical Order Traversal of a Binary Tree

## Problem Statement
Given the root of a binary tree, calculate the vertical order traversal of the binary tree. Nodes at the same horizontal and vertical distance should be sorted by value.

## Approach / Intuition
Perform a [[Breadth-First Search]] using a queue storing `{node, {vertical_dist, level}}`. Use a nested map structure `map<int, map<int, multiset<int>>>` to automatically sort the elements. The outer map keys are the vertical distances, inner map keys are levels, and the multiset handles sorting of values at the exact same position. Extract values sequentially from this map.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N \log N)$ due to multiset and map insertions
- **[[Space Complexity]]:** $O(N)$ for maps and queue

## Sample Code
```cpp
#include <vector>
#include <map>
#include <set>
#include <queue>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

vector<vector<int>> verticalTraversal(TreeNode* root) {
    map<int, map<int, multiset<int>>> nodes;
    queue<pair<TreeNode*, pair<int, int>>> todo;
    
    todo.push({root, {0, 0}});
    while (!todo.empty()) {
        auto p = todo.front();
        todo.pop();
        TreeNode* node = p.first;
        int x = p.second.first;
        int y = p.second.second;
        
        nodes[x][y].insert(node->val);
        
        if (node->left) {
            todo.push({node->left, {x - 1, y + 1}});
        }
        if (node->right) {
            todo.push({node->right, {x + 1, y + 1}});
        }
    }
    
    vector<vector<int>> ans;
    for (auto p : nodes) {
        vector<int> col;
        for (auto q : p.second) {
            col.insert(col.end(), q.second.begin(), q.second.end());
        }
        ans.push_back(col);
    }
    return ans;
}
```

## New Keywords / STL Used
`map`, `multiset`, nested map structures

## Edge Cases
Empty tree, multiple nodes mapping to the exact same vertical and horizontal coordinates.
