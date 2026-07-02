---
type: concept
tags: [dp, cpp, subset_count]
date: 2026-06-30
---
# Subset Count With Sum

## Problem Statement
Find the total number of subsets of a given array that sum up to a specific target value.

## Approach / Intuition
Instead of storing booleans like in [[Subset Sum]], we store counts in our [[Dynamic Programming]] array. The number of ways to make sum `i` is the sum of ways to make `i` without the current element and ways to make `i - num` with it.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n * target)
- **[[Space Complexity]]:** O(target)

## Sample Code
```cpp
int countSubsets(vector<int>& arr, int target) {
    vector<int> dp(target + 1, 0);
    dp[0] = 1;
    for (int num : arr) {
        for (int i = target; i >= num; --i) {
            dp[i] += dp[i - num];
        }
    }
    return dp[target];
}
```

## New Keywords / STL Used
`vector`

## Edge Cases
Zeros in the array (can be included or excluded to make target 0), target is 0, target unreachable.
