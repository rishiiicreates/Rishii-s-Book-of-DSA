---
type: concept
tags: [binary_tree, bst, cpp, graph-conversion]
date: 2026-06-30
---
# Minimum Time to Burn a Tree

## Problem Statement
Given a binary tree and a target node, find the minimum time required to burn the entire tree. The fire spreads to adjacent nodes (parent and children) in 1 second.

## Approach / Intuition
Since fire spreads upwards to the parent as well, the tree needs to be treated as an undirected [[Graph]]. First, perform a traversal to map each node to its parent. Then, use [[Breadth-First Search]] starting from the target node, spreading to the left child, right child, and parent. The total time is the maximum number of BFS levels explored.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(N)$ for parent map, visited map, and queue

## Sample Code
```cpp
#include <unordered_map>
#include <queue>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

TreeNode* mapParents(TreeNode* root, unordered_map<TreeNode*, TreeNode*>& mpp, int start) {
    queue<TreeNode*> q;
    q.push(root);
    TreeNode* res = NULL;
    while (!q.empty()) {
        TreeNode* node = q.front();
        if (node->val == start) res = node;
        q.pop();
        if (node->left) {
            mpp[node->left] = node;
            q.push(node->left);
        }
        if (node->right) {
            mpp[node->right] = node;
            q.push(node->right);
        }
    }
    return res;
}

int minTime(TreeNode* root, int target) {
    unordered_map<TreeNode*, TreeNode*> mpp;
    TreeNode* targetNode = mapParents(root, mpp, target);
    
    unordered_map<TreeNode*, bool> visited;
    queue<TreeNode*> q;
    q.push(targetNode);
    visited[targetNode] = true;
    
    int time = 0;
    while (!q.empty()) {
        int size = q.size();
        bool flag = 0;
        for (int i = 0; i < size; i++) {
            TreeNode* node = q.front();
            q.pop();
            if (node->left && !visited[node->left]) {
                flag = 1;
                visited[node->left] = true;
                q.push(node->left);
            }
            if (node->right && !visited[node->right]) {
                flag = 1;
                visited[node->right] = true;
                q.push(node->right);
            }
            if (mpp[node] && !visited[mpp[node]]) {
                flag = 1;
                visited[mpp[node]] = true;
                q.push(mpp[node]);
            }
        }
        if (flag) time++;
    }
    return time;
}
```

## New Keywords / STL Used
`unordered_map` with pointer keys

## Edge Cases
Target node is the root, tree has only one node.
