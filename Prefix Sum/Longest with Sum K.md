---
type: concept
tags: [prefix sum, cpp, hash-map]
date: 2026-06-30
---
# Longest with Sum K

## Problem Statement
Find the maximum length of a contiguous subarray that sums up to a specific value `K`.

## Approach / Intuition
Maintain a [[Prefix Sum]]. At each step, check if `PrefixSum - K` exists in our [[Hash Map]]. If it does, a subarray with sum `K` ends at the current index. We update the maximum length and ensure we only store the first occurrence of each prefix sum to maximize the length.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the length of the array.
- **[[Space Complexity]]:** O(N) for the hash map.

## Sample Code
```cpp
int longestSubarrayWithSumK(vector<int>& arr, int k) {
    unordered_map<int, int> prefixMap;
    int maxLen = 0;
    int prefixSum = 0;
    for (int i = 0; i < arr.size(); ++i) {
        prefixSum += arr[i];
        if (prefixSum == k) {
            maxLen = i + 1;
        }
        if (prefixMap.find(prefixSum - k) != prefixMap.end()) {
            maxLen = max(maxLen, i - prefixMap[prefixSum - k]);
        }
        if (prefixMap.find(prefixSum) == prefixMap.end()) {
            prefixMap[prefixSum] = i;
        }
    }
    return maxLen;
}
```

## New Keywords / STL Used
- None

## Edge Cases
- Array contains negative numbers and zeros
- No subarray sums to `K`
- Target `K` is 0
