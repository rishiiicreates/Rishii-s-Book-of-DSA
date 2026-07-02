---
type: concept
tags: [sliding window, cpp, deque]
date: 2026-06-30
---
# First negative in every window

## Problem Statement
Given an array of integers and an integer `K`, find the first negative integer encountered in each and every contiguous subarray of size `K`.

## Approach / Intuition
Maintain a deque containing the indices of negative numbers found in the current window in sequential order. When sliding the window, eagerly eject indices that have fallen behind the active window bounds from the front of the deque, and push new negative indices to the back. This highlights an advanced [[Deque Application]] nested inside a sliding window logic flow.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N)
- **[[Space Complexity]]:** O(K)

## Sample Code
```cpp
vector<int> firstNegativeInteger(vector<int>& arr, int k) {
    deque<int> dq;
    vector<int> res;
    for (int i = 0; i < arr.size(); i++) {
        if (!dq.empty() && dq.front() == i - k) {
            dq.pop_front();
        }
        if (arr[i] < 0) {
            dq.push_back(i);
        }
        if (i >= k - 1) {
            if (!dq.empty()) res.push_back(arr[dq.front()]);
            else res.push_back(0);
        }
    }
    return res;
}
```

## New Keywords / STL Used
`std::deque`

## Edge Cases
Arrays entirely absent of negative integers (reporting 0s), `K` matching the full array bounds, universally negative arrays.
