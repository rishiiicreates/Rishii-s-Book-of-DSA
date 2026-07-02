---
type: concept
tags: [bst, cpp]
date: 2026-06-30
---
# Largest BST Subtree

## Problem Statement
Find the largest subtree in a binary tree that is also a valid [[Binary Search Tree]]. Return its size.

## Approach / Intuition
We can use a [[Postorder Traversal]] to process the children before the parent. A node forms a valid BST if both its left and right subtrees are valid BSTs, and its value strictly falls between the maximum of the left subtree and the minimum of the right subtree. We pass up information about validity, size, min, and max.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the number of nodes.
- **[[Space Complexity]]:** O(H) for the recursion stack.

## Sample Code
```cpp
class NodeValue {
public:
    int maxNode, minNode, maxSize;
    
    NodeValue(int minNode, int maxNode, int maxSize) {
        this->maxNode = maxNode;
        this->minNode = minNode;
        this->maxSize = maxSize;
    }
};

class Solution {
    NodeValue largestBSTSubtreeHelper(TreeNode* root) {
        if (!root) {
            return NodeValue(INT_MAX, INT_MIN, 0);
        }
        
        auto left = largestBSTSubtreeHelper(root->left);
        auto right = largestBSTSubtreeHelper(root->right);
        
        if (left.maxNode < root->val && root->val < right.minNode) {
            return NodeValue(
                min(root->val, left.minNode), 
                max(root->val, right.maxNode), 
                left.maxSize + right.maxSize + 1
            );
        }
        
        return NodeValue(INT_MIN, INT_MAX, max(left.maxSize, right.maxSize));
    }

public:
    int largestBSTSubtree(TreeNode* root) {
        return largestBSTSubtreeHelper(root).maxSize;
    }
};
```

## New Keywords / STL Used
- Custom class as return type
- `std::max`, `std::min`
- `INT_MAX`, `INT_MIN`

## Edge Cases
- Entire tree is a BST.
- No subtree greater than a single leaf is a BST.
- Empty tree.
