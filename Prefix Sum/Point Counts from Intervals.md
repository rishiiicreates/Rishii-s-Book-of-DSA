---
type: concept
tags: [prefix sum, cpp, difference-array]
date: 2026-06-30
---
# Point Counts from Intervals

## Problem Statement
Given a set of intervals `[L, R]`, find the maximum number of overlapping intervals at any point.

## Approach / Intuition
Use a [[Difference Array]] technique. For each interval `[L, R]`, increment the value at index `L` and decrement at index `R + 1`. Then, compute the [[Prefix Sum]] of this array to find the number of overlapping intervals at any given point, and find the maximum of these prefix sums.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N + M) where N is the number of intervals and M is the range of coordinates.
- **[[Space Complexity]]:** O(M) for the difference array.

## Sample Code
```cpp
int maxOverlappingIntervals(vector<pair<int, int>>& intervals, int max_val) {
    vector<int> diff(max_val + 2, 0);
    for (auto& interval : intervals) {
        diff[interval.first]++;
        diff[interval.second + 1]--;
    }
    
    int maxOverlap = 0;
    int currentOverlap = 0;
    for (int i = 0; i <= max_val; ++i) {
        currentOverlap += diff[i];
        maxOverlap = max(maxOverlap, currentOverlap);
    }
    return maxOverlap;
}
```

## New Keywords / STL Used
- `std::pair`

## Edge Cases
- No intervals
- Overlapping at boundaries
- Large coordinate range (requires coordinate compression)
