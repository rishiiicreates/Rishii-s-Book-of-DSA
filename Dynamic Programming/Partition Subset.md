---
type: concept
tags: [dp, cpp, subset_sum]
date: 2026-06-30
---
# Partition Subset

## Problem Statement
Determine if a given array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

## Approach / Intuition
The problem reduces to finding if there exists a subset with a sum equal to half of the total array sum. We can use [[Dynamic Programming]] to solve the [[Subset Sum]] problem where the target is exactly half the total sum.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n * target)
- **[[Space Complexity]]:** O(target)

## Sample Code
```cpp
bool canPartition(vector<int>& nums) {
    int sum = 0;
    for (int num : nums) sum += num;
    if (sum % 2 != 0) return false;
    int target = sum / 2;
    vector<bool> dp(target + 1, false);
    dp[0] = true;
    for (int num : nums) {
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
Array sum is odd, array length is 1, elements sum up to target directly.
