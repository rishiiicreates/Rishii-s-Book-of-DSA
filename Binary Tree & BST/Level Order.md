---
type: concept
tags: [binary_tree, bst, cpp, bfs]
date: 2026-06-30
---
# Level Order

## Problem Statement
Traverse a binary tree level by level, from top to bottom and left to right.

## Approach / Intuition
Use a [[Queue]] to implement [[Breadth First Search]]. Push the root, then loop until the queue is empty: pop a node, process it, and push its non-null children.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N)
- **[[Space Complexity]]:** O(W) where W is the maximum width of the tree (up to N/2).

## Sample Code
```cpp
vector<int> levelOrder(TreeNode* root) {
    vector<int> res;
    if (!root) return res;
    queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        TreeNode* curr = q.front();
        q.pop();
        res.push_back(curr->val);
        if (curr->left) q.push(curr->left);
        if (curr->right) q.push(curr->right);
    }
    return res;
}
```

## New Keywords / STL Used
- `std::queue`, `q.front()`, `q.pop()`, `q.push()`

## Edge Cases
- Empty tree
- Perfect binary tree (maximum queue size)
