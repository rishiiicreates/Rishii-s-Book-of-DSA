# Point Counts from Intervals

## Problem Statement
- given a set of intervals `[L, R]`, find the maximum number of overlapping intervals at any point.

## Approach / Intuition
- use a [[Difference Array]] technique. For each interval `[L, R]`, increment the value at index `L` and decrement at index `R + 1`. Then, compute the [[Prefix Sum]] of this array to find the number of overlapping intervals at any given point, and find the maximum of these prefix sums.

## Time & Space Complexity
- **[[time Complexity]]:** O(N + M) where N is the number of intervals and M is the range of coordinates.
- **[[space Complexity]]:** O(M) for the difference array.

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
- no intervals
- overlapping at boundaries
- large coordinate range (requires coordinate compression)

NEXT: [[Index]]
