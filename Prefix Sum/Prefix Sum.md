---
type: concept
tags: [prefix sum, cpp, arrays]
date: 2026-06-30
---
# Prefix Sum

## Problem Statement
Calculate the prefix sum of an array, where the value at index $i$ is the sum of all elements from index $0$ to $i$.

## Approach / Intuition
The core idea of a [[Prefix Sum]] is to precompute cumulative sums so that we can answer subsequent range sum queries efficiently. We iterate through the array, adding the current element to a running total. This running total represents the sum of all elements up to the current index. 

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(1)$ auxiliary, $O(N)$ for output

## Sample Code
```cpp
#include <vector>

std::vector<int> computePrefixSum(const std::vector<int>& arr) {
    if (arr.empty()) return {};
    std::vector<int> pref(arr.size());
    pref[0] = arr[0];
    for (int i = 1; i < arr.size(); i++) {
        pref[i] = pref[i - 1] + arr[i];
    }
    return pref;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
- Empty array
- Array with negative numbers
- Large sums that might cause integer overflow
