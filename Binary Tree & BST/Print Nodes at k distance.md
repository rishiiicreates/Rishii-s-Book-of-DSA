---
type: concept
tags: [binary_tree, bst, cpp, dfs]
date: 2026-06-30
---
# Print Nodes at k distance

## Problem Statement
Print or collect all nodes that are exactly at a distance `k` from the root of a binary tree.

## Approach / Intuition
Traverse the tree using [[Depth First Search]]. Keep track of the current depth. When the current depth equals `k`, add the node's value to the result and stop exploring that branch further.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the number of nodes.
- **[[Space Complexity]]:** O(H) for the recursion stack.

## Sample Code
```cpp
void printKDistance(TreeNode* root, int k, vector<int>& res) {
    if (!root || k < 0) return;
    if (k == 0) {
        res.push_back(root->val);
        return;
    }
    printKDistance(root->left, k - 1, res);
    printKDistance(root->right, k - 1, res);
}
```

## New Keywords / STL Used
- None

## Edge Cases
- Empty tree
- `k` is greater than the height of the tree
- `k` is 0 (returns just the root)
