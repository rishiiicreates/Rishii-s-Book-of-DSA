---
type: concept
tags: [prefix sum, cpp, longest-span]
date: 2026-06-30
---
# Longest Span with Same Sum in Two Binary Arrays

## Problem Statement
Given two binary arrays `arr1` and `arr2` of the same size, find the length of the longest span (subarray) where the sum of elements in `arr1` perfectly equals the sum of elements in `arr2`.

## Approach / Intuition
Create a unified difference evaluation where `diff = arr1[i] - arr2[i]`. The problem then scales identically to finding the longest subarray with a total sum of 0. Track running totals leveraging a [[Prefix Sum]] hash map to cache the earliest seen indices for maximum span calculation.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N)
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
int longestCommonSumSpan(vector<int>& arr1, vector<int>& arr2) {
    int n = arr1.size();
    unordered_map<int, int> prefixIndices;
    int maxLen = 0, currentSum = 0;
    prefixIndices[0] = -1;
    for (int i = 0; i < n; i++) {
        currentSum += arr1[i] - arr2[i];
        if (prefixIndices.find(currentSum) != prefixIndices.end()) {
            maxLen = max(maxLen, i - prefixIndices[currentSum]);
        } else {
            prefixIndices[currentSum] = i;
        }
    }
    return maxLen;
}
```

## New Keywords / STL Used
`std::unordered_map`

## Edge Cases
No common sum span exists anywhere (returns 0), the entire combined array perfectly mirrors parity, highly imbalanced zero/one arrays.
