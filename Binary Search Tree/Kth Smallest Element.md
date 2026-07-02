---
type: concept
tags: [bst, cpp]
date: 2026-06-30
---
# Kth Smallest Element

## Problem Statement
Find the kth smallest element in a [[Binary Search Tree]].

## Approach / Intuition
Perform an inorder traversal which visits the nodes in ascending order. Maintain a count of visited nodes and stop when the count reaches `k`. This approach leverages the [[Inorder Traversal]] property of BSTs.

## Time & Space Complexity
- **[[Time Complexity]]:** O(H + K) where H is the height of the tree.
- **[[Space Complexity]]:** O(H) for the recursion stack.

## Sample Code
```cpp
class Solution {
    int count = 0;
    int result = -1;

    void inorder(TreeNode* root, int k) {
        if (!root) return;
        
        inorder(root->left, k);
        
        count++;
        if (count == k) {
            result = root->val;
            return;
        }
        
        inorder(root->right, k);
    }

public:
    int kthSmallest(TreeNode* root, int k) {
        inorder(root, k);
        return result;
    }
};
```

## New Keywords / STL Used
- Class member variables

## Edge Cases
- `k` is 1 (minimum element).
- `k` is equal to the number of nodes in the tree.
- Tree has only one node.
