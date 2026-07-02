---
type: concept
tags: [binary_tree, bst, cpp, tree-traversal]
date: 2026-06-30
---
# Spiral (Zig-Zag) Traversal of Binary Tree

## Problem Statement
Return the zigzag level order traversal of a binary tree's nodes' values (i.e., from left to right, then right to left for the next level and alternate between).

## Approach / Intuition
Perform a [[Breadth-First Search]] using a queue. For each level, determine the number of elements and instantiate an [[Array]] of that size. Based on a boolean flag indicating the direction (left-to-right or right-to-left), place the dequeued nodes into the array either starting from the front or the back. Toggle the flag after each level.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <vector>
#include <queue>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    vector<vector<int>> result;
    if (!root) return result;
    
    queue<TreeNode*> q;
    q.push(root);
    bool leftToRight = true;
    
    while (!q.empty()) {
        int size = q.size();
        vector<int> row(size);
        for (int i = 0; i < size; i++) {
            TreeNode* node = q.front();
            q.pop();
            
            int index = leftToRight ? i : (size - 1 - i);
            row[index] = node->val;
            
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        leftToRight = !leftToRight;
        result.push_back(row);
    }
    return result;
}
```

## New Keywords / STL Used
`queue`, vector pre-sizing

## Edge Cases
Empty tree, single node.
