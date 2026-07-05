# Longest with Sum K

## Problem Statement
- find the maximum length of a contiguous subarray that sums up to a specific value `K`.

## Approach / Intuition
- maintain a [[Prefix Sum]]. At each step, check if `PrefixSum - K` exists in our [[Hash Map]]. If it does, a subarray with sum `K` ends at the current index. We update the maximum length and ensure we only store the first occurrence of each prefix sum to maximize the length.

## Time & Space Complexity
- **[[time Complexity]]:** O(N) where N is the length of the array.
- **[[space Complexity]]:** O(N) for the hash map.

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
- none

## Edge Cases
- array contains negative numbers and zeros
- no subarray sums to `K`
- target `K` is 0

NEXT: [[Index]]
