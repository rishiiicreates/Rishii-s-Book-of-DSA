---
type: concept
tags: [bst, cpp]
date: 2026-06-30
---
# 2 Sum in BST

## Problem Statement
Determine if there exist two elements in a [[Binary Search Tree]] such that their sum is equal to a given target.

## Approach / Intuition
We can use a [[BST Iterator]] or simply perform an inorder traversal to store the elements in a sorted array. Then, use the [[Two Pointer]] approach on the sorted array to find if a pair sums up to the target. Alternatively, use a [[Hash Set]] during traversal.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the number of nodes.
- **[[Space Complexity]]:** O(N) to store the elements in an array or set.

## Sample Code
```cpp
class Solution {
    void inorder(TreeNode* root, vector<int>& nums) {
        if (!root) return;
        inorder(root->left, nums);
        nums.push_back(root->val);
        inorder(root->right, nums);
    }

public:
    bool findTarget(TreeNode* root, int k) {
        vector<int> nums;
        inorder(root, nums);
        
        int left = 0, right = nums.size() - 1;
        while (left < right) {
            int sum = nums[left] + nums[right];
            if (sum == k) return true;
            if (sum < k) left++;
            else right--;
        }
        return false;
    }
};
```

## New Keywords / STL Used
- `std::vector`
- `push_back`

## Edge Cases
- Tree has fewer than two nodes.
- Multiple pairs exist.
- Target is negative or very large.
