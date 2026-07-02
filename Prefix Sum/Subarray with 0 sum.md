---
type: concept
tags: [prefix sum, cpp, hash-set]
date: 2026-06-30
---
# Subarray with 0 sum

## Problem Statement
Determine if there exists a contiguous subarray with a sum equal to 0.

## Approach / Intuition
Compute the [[Prefix Sum]] as you iterate through the array. If the prefix sum becomes 0, or if a prefix sum value is repeated (which we can check using a [[Hash Set]]), it means the elements between the two identical prefix sums add up to 0.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the length of the array.
- **[[Space Complexity]]:** O(N) to store prefix sums.

## Sample Code
```cpp
bool hasZeroSumSubarray(vector<int>& arr) {
    unordered_set<int> seenSums;
    int prefixSum = 0;
    for (int num : arr) {
        prefixSum += num;
        if (prefixSum == 0 || seenSums.find(prefixSum) != seenSums.end()) {
            return true;
        }
        seenSums.insert(prefixSum);
    }
    return false;
}
```

## New Keywords / STL Used
- `std::unordered_set`

## Edge Cases
- Array contains a single 0
- Entire array sums to 0
- No such subarray exists
