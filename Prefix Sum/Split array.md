---
type: concept
tags: [prefix sum, cpp, partitioning]
date: 2026-06-30
---
# Split array

## Problem Statement
Determine if it is possible to split an array into two subarrays such that the sum of elements in the left subarray is equal to the sum of elements in the right subarray.

## Approach / Intuition
This requires finding an index where the [[Prefix Sum]] up to that index equals the sum of the remaining elements. We first compute the total sum of the array. Then we iterate through the array, keeping a running sum. At each step, if the running sum is exactly half of the total sum, the array can be split evenly at this point.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(1)$

## Sample Code
```cpp
#include <vector>
#include <numeric>

bool canSplitEqually(std::vector<int>& arr) {
    long long total = std::accumulate(arr.begin(), arr.end(), 0LL);
    
    if (total % 2 != 0) return false;
    
    long long running_sum = 0;
    for (int i = 0; i < arr.size() - 1; i++) {
        running_sum += arr[i];
        if (running_sum == total / 2) {
            return true;
        }
    }
    
    return false;
}
```

## New Keywords / STL Used
`std::accumulate`

## Edge Cases
- Odd total sum (impossible to split into integer sums)
- Arrays with zeros or negative numbers leading to multiple valid split points
- Empty or single-element arrays
