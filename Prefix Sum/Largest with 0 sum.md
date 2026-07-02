---
type: concept
tags: [prefix sum, cpp, hash-map]
date: 2026-06-30
---
# Largest with 0 sum

## Problem Statement
Find the length of the largest continuous subarray that sums to 0.

## Approach / Intuition
Maintain a [[Prefix Sum]] and store its first occurrence in a [[Hash Map]] along with its index. If a prefix sum is seen again, the subarray from the stored index to the current index has a sum of 0. Update the maximum length accordingly.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the length of the array.
- **[[Space Complexity]]:** O(N) for the hash map.

## Sample Code
```cpp
int maxLenZeroSum(vector<int>& arr) {
    unordered_map<int, int> prefixMap;
    int maxLen = 0;
    int prefixSum = 0;
    for (int i = 0; i < arr.size(); ++i) {
        prefixSum += arr[i];
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
- `std::unordered_map`, `std::max`

## Edge Cases
- All elements are positive
- All elements are 0
- Entire array sums to 0
