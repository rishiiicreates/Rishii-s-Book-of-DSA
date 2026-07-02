---
type: concept
tags: [prefix sum, cpp, arrays]
date: 2026-06-30
---
# Equilibrium Index

## Problem Statement
Find the equilibrium index of an array. An equilibrium index is an index such that the sum of elements at lower indexes is equal to the sum of elements at higher indexes.

## Approach / Intuition
We can solve this efficiently using the [[Prefix Sum]] concept. First, compute the total sum of the array. Then, iterate through the array while maintaining a running `left_sum`. The `right_sum` at any index $i$ can be calculated as `total_sum - left_sum - arr[i]`. If `left_sum == right_sum`, we have found our equilibrium index.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(1)$

## Sample Code
```cpp
#include <vector>
#include <numeric>

int findEquilibriumIndex(std::vector<int>& arr) {
    long long total_sum = std::accumulate(arr.begin(), arr.end(), 0LL);
    long long left_sum = 0;
    
    for (int i = 0; i < arr.size(); i++) {
        long long right_sum = total_sum - left_sum - arr[i];
        if (left_sum == right_sum) {
            return i;
        }
        left_sum += arr[i];
    }
    
    return -1;
}
```

## New Keywords / STL Used
`std::accumulate`, `0LL`

## Edge Cases
- Array where equilibrium index is at 0 (left sum is 0) or at the end (right sum is 0)
- No equilibrium index exists
- Negative numbers resulting in an equilibrium
