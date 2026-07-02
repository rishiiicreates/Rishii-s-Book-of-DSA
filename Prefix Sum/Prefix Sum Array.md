---
type: concept
tags: [prefix sum, cpp, arrays]
date: 2026-06-30
---
# Prefix Sum Array

## Problem Statement
Given an integer array, return a new array such that each element at index $i$ of the new array is the sum of all elements of the original array up to index $i$.

## Approach / Intuition
We construct a [[Prefix Sum Array]] by taking the element at the previous index in our prefix sum array and adding the current element from the original array. The value at `prefix[i]` will be `prefix[i-1] + arr[i]`. This provides an $O(1)$ lookup for the sum of any prefix of the array.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(N)$ to store the array

## Sample Code
```cpp
#include <vector>

std::vector<int> getPrefixSumArray(std::vector<int>& nums) {
    int n = nums.size();
    std::vector<int> prefix(n, 0);
    if (n == 0) return prefix;
    
    prefix[0] = nums[0];
    for (int i = 1; i < n; i++) {
        prefix[i] = prefix[i - 1] + nums[i];
    }
    
    return prefix;
}
```

## New Keywords / STL Used
`std::vector` initialization

## Edge Cases
- Empty array
- Single element array
- Very large integer sums requiring `long long`
