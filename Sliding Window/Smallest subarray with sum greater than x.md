---
type: concept
tags: [sliding window, cpp, prefix-sum]
date: 2026-06-30
---
# Smallest subarray with sum greater than x

## Problem Statement
Find the minimum length of a contiguous subarray whose sum is strictly greater than a given value `x`.

## Approach / Intuition
Utilize a [[Sliding Window]]. Maintain a running sum as the right pointer expands. When the sum exceeds `x`, record the window size and continuously shrink from the left to find the smallest possible valid window.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the size of the array.
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
int smallestSubWithSum(vector<int>& arr, int x) {
    int left = 0, currentSum = 0, minLen = INT_MAX;
    for (int right = 0; right < arr.size(); ++right) {
        currentSum += arr[right];
        while (currentSum > x) {
            minLen = min(minLen, right - left + 1);
            currentSum -= arr[left];
            left++;
        }
    }
    return minLen == INT_MAX ? 0 : minLen;
}
```

## New Keywords / STL Used
- `INT_MAX`, `std::min`

## Edge Cases
- Entire array sum is less than or equal to `x`
- Single element is greater than `x`
- Empty array
