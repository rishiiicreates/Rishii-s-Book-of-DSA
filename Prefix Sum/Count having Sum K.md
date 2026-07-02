---
type: concept
tags: [prefix sum, cpp, hash-map]
date: 2026-06-30
---
# Count having Sum K

## Problem Statement
Count the total number of continuous subarrays whose sum equals exactly `K`.

## Approach / Intuition
Maintain a running [[Prefix Sum]]. Use a [[Hash Map]] to store the frequency of all prefix sums encountered. At each step, if `PrefixSum - K` exists in the map, it means there are valid subarrays ending at the current position. Add its frequency to the total count.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the length of the array.
- **[[Space Complexity]]:** O(N) for storing prefix sum frequencies.

## Sample Code
```cpp
int subarraySum(vector<int>& nums, int k) {
    unordered_map<int, int> prefixCounts;
    prefixCounts[0] = 1;
    int count = 0, prefixSum = 0;
    for (int num : nums) {
        prefixSum += num;
        if (prefixCounts.find(prefixSum - k) != prefixCounts.end()) {
            count += prefixCounts[prefixSum - k];
        }
        prefixCounts[prefixSum]++;
    }
    return count;
}
```

## New Keywords / STL Used
- None

## Edge Cases
- Array contains negative numbers and zeros
- Subarray can be a single element
- Target `K` is 0
