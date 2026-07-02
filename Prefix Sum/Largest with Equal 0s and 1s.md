---
type: concept
tags: [prefix sum, cpp, hash-map]
date: 2026-06-30
---
# Largest with Equal 0s and 1s

## Problem Statement
Given a binary array, find the maximum length of a contiguous subarray with an equal number of 0s and 1s.

## Approach / Intuition
Replace all 0s in the array with -1s. The problem then reduces to finding the largest subarray with a sum of 0. We can solve this using a [[Prefix Sum]] and a [[Hash Map]] to store the first occurrence of each prefix sum.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the length of the array.
- **[[Space Complexity]]:** O(N) for the hash map.

## Sample Code
```cpp
int findMaxLength(vector<int>& nums) {
    unordered_map<int, int> prefixMap;
    int maxLen = 0;
    int prefixSum = 0;
    for (int i = 0; i < nums.size(); ++i) {
        prefixSum += (nums[i] == 0) ? -1 : 1;
        if (prefixSum == 0) {
            maxLen = i + 1;
        } else if (prefixMap.find(prefixSum) != prefixMap.end()) {
            maxLen = max(maxLen, i - prefixMap[prefixSum]);
        } else {
            prefixMap[prefixSum] = i;
        }
    }
    return maxLen;
}
```

## New Keywords / STL Used
- `std::unordered_map`

## Edge Cases
- Array contains only 1s or only 0s
- Empty array
- Array has an equal number of 0s and 1s entirely
