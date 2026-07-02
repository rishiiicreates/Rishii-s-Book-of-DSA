---
type: concept
tags: [sliding window, cpp, max-xor, bit-manipulation]
date: 2026-06-30
---
# Max XOR of K size

## Problem Statement
Given an array of integers and a number `K`, find the maximum bitwise XOR of any contiguous subarray of exactly size `K`.

## Approach / Intuition
Calculate the XOR of the first `K` elements. Because the bitwise XOR operator is its own inverse (`A ^ A = 0`), you can slide the window efficiently by XORing the current window's aggregate value with the element entering the window and the element leaving the window. This perfectly aligns [[Sliding Window]] logic with bitwise math.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N)
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
int maxXorSubarray(vector<int>& arr, int k) {
    if (k > arr.size()) return 0;
    int currentXor = 0, maxXor = 0;
    for (int i = 0; i < k; i++) {
        currentXor ^= arr[i];
    }
    maxXor = currentXor;
    for (int i = k; i < arr.size(); i++) {
        currentXor ^= arr[i] ^ arr[i - k];
        maxXor = max(maxXor, currentXor);
    }
    return maxXor;
}
```

## New Keywords / STL Used
`^`

## Edge Cases
Subarray size larger than array size, array populated exclusively with identical elements leading to zeroed XOR outcomes.
