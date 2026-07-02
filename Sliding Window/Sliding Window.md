---
type: concept
tags: [sliding window, cpp, basics]
date: 2026-06-30
---
# Sliding Window

## Problem Statement
Solve problems involving contiguous subarrays or subsegments of a fixed or variable size `K` optimally.

## Approach / Intuition
Instead of recalculating the sum or state of the window from scratch for every position, maintain the current window state dynamically. Update it by adding the new element that enters the window and removing the old element that exits. This transforms a naive O(N*K) approach into a highly optimal O(N) [[Sliding Window]] traversal.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N)
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
int genericSlidingWindow(vector<int>& arr, int k) {
    int sum = 0, max_sum = 0;
    int n = arr.size();
    if (k > n) return 0;
    for (int i = 0; i < k; i++) sum += arr[i];
    max_sum = sum;
    for (int i = k; i < n; i++) {
        sum += arr[i] - arr[i - k];
        max_sum = max(max_sum, sum);
    }
    return max_sum;
}
```

## New Keywords / STL Used
`std::max`

## Edge Cases
`K > N`, `K <= 0`, empty array, completely zeroed arrays.
