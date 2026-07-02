---
type: concept
tags: [sliding window, cpp, max-consecutive]
date: 2026-06-30
---
# Maximize Ones

## Problem Statement
Find the maximum number of consecutive 1s in a binary array if you can flip at most `k` 0s.

## Approach / Intuition
Use a [[Sliding Window]] to maintain a sequence of 1s. Expand the window to the right and keep a count of zeros. If the zero count exceeds `k`, shrink the window from the left until the count is valid again.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the length of the array.
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
int longestOnes(vector<int>& nums, int k) {
    int left = 0, maxLen = 0, zeros = 0;
    for (int right = 0; right < nums.size(); ++right) {
        if (nums[right] == 0) zeros++;
        while (zeros > k) {
            if (nums[left] == 0) zeros--;
            left++;
        }
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

## New Keywords / STL Used
- `std::max`

## Edge Cases
- All 1s or all 0s in the array
- `k` is 0
- Array is empty
