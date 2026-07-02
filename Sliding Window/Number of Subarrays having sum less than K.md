---
type: concept
tags: [sliding window, cpp, sum]
date: 2026-06-30
---
# Number of Subarrays having sum less than K

## Problem Statement
Given an array of non-negative integers and a target sum `k`, find the number of contiguous subarrays whose sum is strictly less than `k`.

## Approach / Intuition
Since all elements are non-negative, the sum monotonically increases as the window expands. Use a [[Sliding Window]] to track the running sum. If the sum exceeds or equals `k`, increment the `left` pointer until the sum is valid again. Add `right - left + 1` to the total count for each valid configuration.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n)
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
int numSubarraysWithSumLessThanK(const vector<int>& nums, int k) {
    int sum = 0, count = 0, left = 0;
    for (int right = 0; right < nums.size(); ++right) {
        sum += nums[right];
        while (sum >= k && left <= right) {
            sum -= nums[left];
            left++;
        }
        count += (right - left + 1);
    }
    return count;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
`k <= 0`, empty array, all elements greater than `k`.
