---
type: concept
tags: [prefix sum, cpp, math]
date: 2026-06-30
---
# Largest with sum divisible by k

## Problem Statement
Find the length of the largest continuous subarray whose sum is a multiple of `k`.

## Approach / Intuition
Compute the [[Prefix Sum]] modulo `k` at each step. If the same remainder appears at two different indices, the subarray between them has a sum divisible by `k`. Use a [[Hash Map]] to store the first occurrence of each remainder. Remember to handle negative remainders correctly.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the number of elements.
- **[[Space Complexity]]:** O(min(N, K)) for the hash map.

## Sample Code
```cpp
int maxLenDivisibleByK(vector<int>& arr, int k) {
    unordered_map<int, int> remMap;
    int maxLen = 0;
    int prefixSum = 0;
    for (int i = 0; i < arr.size(); ++i) {
        prefixSum += arr[i];
        int rem = prefixSum % k;
        if (rem < 0) rem += k;
        
        if (rem == 0) {
            maxLen = i + 1;
        } else if (remMap.find(rem) != remMap.end()) {
            maxLen = max(maxLen, i - remMap[rem]);
        } else {
            remMap[rem] = i;
        }
    }
    return maxLen;
}
```

## New Keywords / STL Used
- `std::unordered_map`

## Edge Cases
- All elements are negative
- `k` is 1 (entire array is the answer)
- No subarray is divisible by `k`
