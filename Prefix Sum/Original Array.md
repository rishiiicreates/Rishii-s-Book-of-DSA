---
type: concept
tags: [prefix sum, cpp, inverse]
date: 2026-06-30
---
# Find Original Array from Prefix Sum

## Problem Statement
Given a prefix sum array, reconstruct and return the original array.

## Approach / Intuition
Since `prefix[i] = prefix[i-1] + arr[i]`, we can mathematically invert this logic. The first element naturally remains identical, and every subsequent element `arr[i]` can be extracted by performing `prefix[i] - prefix[i-1]`. This serves as the foundational reverse operation of a standard [[Prefix Sum]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(N)
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
vector<int> findOriginalArray(vector<int>& prefix) {
    int n = prefix.size();
    if (n == 0) return {};
    vector<int> arr(n);
    arr[0] = prefix[0];
    for (int i = 1; i < n; i++) {
        arr[i] = prefix[i] - prefix[i-1];
    }
    return arr;
}
```

## New Keywords / STL Used
None

## Edge Cases
Empty prefix array, prefix array of size 1, arrays carrying negative aggregated totals.
