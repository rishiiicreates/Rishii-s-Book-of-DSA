---
type: concept
tags: [bst, cpp]
date: 2026-06-30
---
# Sorted Array to BST

## Problem Statement
Convert a given sorted array into a height-balanced [[Binary Search Tree]].

## Approach / Intuition
A height-balanced BST requires the middle element of the sorted array to be the root. The left half of the array forms the left subtree and the right half forms the right subtree. We can build the tree recursively using [[Divide and Conquer]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the number of elements in the array.
- **[[Space Complexity]]:** O(log N) for the recursion stack.

## Sample Code
```cpp
class Solution {
    TreeNode* buildBST(const vector<int>& nums, int left, int right) {
        if (left > right) return nullptr;
        
        int mid = left + (right - left) / 2;
        TreeNode* root = new TreeNode(nums[mid]);
        
        root->left = buildBST(nums, left, mid - 1);
        root->right = buildBST(nums, mid + 1, right);
        
        return root;
    }

public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return buildBST(nums, 0, nums.size() - 1);
    }
};
```

## New Keywords / STL Used
- `const std::vector<int>&`

## Edge Cases
- Empty array.
- Array with a single element.
- Array with an even number of elements.
