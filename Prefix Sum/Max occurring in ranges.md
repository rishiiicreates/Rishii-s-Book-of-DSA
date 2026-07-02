---
type: concept
tags: [prefix sum, cpp, range-queries]
date: 2026-06-30
---
# Max Occurring Element in Given Ranges

## Problem Statement
Given multiple ranges `[L_i, R_i]`, efficiently find the single integer element that occurs the maximum number of times across all combined intervals.

## Approach / Intuition
Because interval ranges imply contiguous increments, leverage a simple [[Difference Array]]. Add `1` precisely at boundary `L_i` and subtract `1` right after the boundary at `R_i + 1`. Execute a subsequent [[Prefix Sum]] sweep across the timeline space to naturally unfold actual frequencies, logging the index when the maximum frequency is achieved.

## Time & Space Complexity
- **[[Time Complexity]]:** O(Max(R_i) + N)
- **[[Space Complexity]]:** O(Max(R_i))

## Sample Code
```cpp
int maxOccurring(vector<int>& L, vector<int>& R) {
    int max_val = 0;
    for (int r : R) max_val = max(max_val, r);
    vector<int> diff(max_val + 2, 0);
    for (int i = 0; i < L.size(); i++) {
        diff[L[i]]++;
        diff[R[i] + 1]--;
    }
    int max_freq = diff[0], res = 0;
    for (int i = 1; i <= max_val; i++) {
        diff[i] += diff[i - 1];
        if (diff[i] > max_freq) {
            max_freq = diff[i];
            res = i;
        }
    }
    return res;
}
```

## New Keywords / STL Used
None

## Edge Cases
Wildly vast coordinate spaces causing excessive memory usage, strictly disconnected ranges.
