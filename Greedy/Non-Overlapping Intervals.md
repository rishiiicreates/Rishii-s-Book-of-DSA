# Non-Overlapping Intervals

## Problem Statement
- given a collection of intervals, find the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

## Approach / Intuition
- this is equivalent to finding the maximum number of non-overlapping intervals (like Activity Selection). Sort the intervals by their end times. Use a [[Greedy Algorithm]] to keep intervals that end earliest, thereby leaving more space for subsequent intervals.

## Time & Space Complexity
- **[[time Complexity]]:** O(N log N)
- **[[space Complexity]]:** O(1) or O(N) depending on sorting

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    if (intervals.empty()) return 0;
    sort(intervals.begin(), intervals.end(), [](const vector<int>& a, const vector<int>& b) {
        return a[1] < b[1];
    });
    int count = 1;
    int end = intervals[0][1];
    for (int i = 1; i < intervals.size(); i++) {
        if (intervals[i][0] >= end) {
            count++;
            end = intervals[i][1];
        }
    }
    return intervals.size() - count;
}
```

## New Keywords / STL Used
- lambda expression for custom sorting of vectors

## Edge Cases
- intervals fully contained within other intervals
- identical intervals

NEXT: [[Index]]
