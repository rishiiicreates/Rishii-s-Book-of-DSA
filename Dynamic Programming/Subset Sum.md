---
type: concept
tags: [dp, cpp, subset_sum]
date: 2026-06-30
---
# Subset Sum

## Problem Statement
Given an array of non-negative integers and a target sum, determine if there is a subset of the given set with a sum equal to the given target.

## Approach / Intuition
Use a 1D [[Dynamic Programming]] array to keep track of achievable sums. For each element, iterate backwards through the target values to avoid using the same element multiple times, building up possible subset sums.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n * target)
- **[[Space Complexity]]:** O(target)

## Sample Code
```cpp
bool isSubsetSum(vector<int>& arr, int target) {
    vector<bool> dp(target + 1, false);
    dp[0] = true;
    for (int num : arr) {
        for (int i = target; i >= num; --i) {
            dp[i] = dp[i] | dp[i - num];
        }
    }
    return dp[target];
}
```

## New Keywords / STL Used
`vector`

## Edge Cases
Target is 0 (empty subset always works), all elements larger than target.
