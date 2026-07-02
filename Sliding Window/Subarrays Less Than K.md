---
type: concept
tags: [sliding window, cpp, product]
date: 2026-06-30
---
# Subarray Product Less Than K

## Problem Statement
Given an array of positive integers and an integer `k`, return the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than `k`.

## Approach / Intuition
Use a [[Sliding Window]] to maintain a running product. As the `right` pointer expands the window, multiply the current element. If the product reaches or exceeds `k`, shrink the window from the `left` until the product is less than `k` again. At each valid step, the number of valid subarrays ending at `right` is `right - left + 1`.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n)
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
int numSubarrayProductLessThanK(const vector<int>& nums, int k) {
    if (k <= 1) return 0;
    int prod = 1, res = 0, left = 0;
    for (int right = 0; right < nums.size(); ++right) {
        prod *= nums[right];
        while (prod >= k) {
            prod /= nums[left];
            left++;
        }
        res += (right - left + 1);
    }
    return res;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
`k` is 0 or 1, all elements larger than `k`, empty array.
