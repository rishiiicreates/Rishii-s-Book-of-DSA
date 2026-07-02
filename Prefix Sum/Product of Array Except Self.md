---
type: concept
tags: [prefix sum, cpp, prefix-product]
date: 2026-06-30
---
# Product of Array Except Self

## Problem Statement
Given an array of integers, return an array such that each element at index `i` is the product of all the elements of the original array except `arr[i]`, strictly without using the division operator.

## Approach / Intuition
Calculate the left cumulative [[Prefix Product]] up to each element in a forward pass. In a backward pass, calculate the right cumulative suffix product and multiply it with the left products. This computes the surrounding product dynamically without relying on division.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N)
- **[[Space Complexity]]:** O(1) auxiliary space (excluding output array)

## Sample Code
```cpp
vector<int> productExceptSelf(vector<int>& nums) {
    int n = nums.size();
    vector<int> res(n, 1);
    int left = 1;
    for (int i = 0; i < n; i++) {
        res[i] = left;
        left *= nums[i];
    }
    int right = 1;
    for (int i = n - 1; i >= 0; i--) {
        res[i] *= right;
        right *= nums[i];
    }
    return res;
}
```

## New Keywords / STL Used
None

## Edge Cases
Array containing a single zero (product relies entirely on suffix/prefix), array containing multiple zeros (all outputs evaluate to zero).
