---
type: concept
tags: [sliding window, cpp, prefix-sum]
date: 2026-06-30
---
# Subarray with given sum

## Problem Statement
Given an array of non-negative integers, find a continuous subarray that adds up to a given target sum.

## Approach / Intuition
Use a [[Sliding Window]] approach. Add elements to a running sum by moving the right pointer. While the sum exceeds the target, subtract elements from the left. This works efficiently because all numbers are non-negative.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the number of elements.
- **[[Space Complexity]]:** O(1) auxiliary space.

## Sample Code
```cpp
vector<int> subarraySum(vector<int>& arr, int target) {
    int left = 0, currentSum = 0;
    for (int right = 0; right < arr.size(); ++right) {
        currentSum += arr[right];
        while (currentSum > target && left <= right) {
            currentSum -= arr[left];
            left++;
        }
        if (currentSum == target) {
            return {left, right};
        }
    }
    return {-1};
}
```

## New Keywords / STL Used
- None

## Edge Cases
- Target is 0
- No subarray matches the target
- Array contains zeros
