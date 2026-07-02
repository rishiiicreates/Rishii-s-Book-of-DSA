---
type: concept
tags: [heap, dsa, cpp]
date: 2026-06-30
---
# The Skyline Problem

## Problem Statement
Given the locations and heights of several buildings, return the skyline formed by these buildings collectively.

## Approach / Intuition
Represent each building as two events: a start event (x, height, entering) and an end event (x, height, leaving). Sort the events primarily by the `x` coordinate. We process the events and maintain the active buildings' heights in a max-[[Heap]] (or a `multiset` allowing lazy deletion / log(N) removal). For each event, if it's a start, add its height. If it's an end, remove its height. After processing all events at a given `x`, if the maximum active height changes from the previous maximum, we record a new key point for the skyline.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log N) for sorting and heap/multiset operations.
- **[[Space Complexity]]:** O(N) to store events and active heights.

## Sample Code
```cpp
#include <vector>
#include <set>
#include <algorithm>

using namespace std;

vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
    vector<pair<int, int>> events;
    for (auto& b : buildings) {
        events.push_back({b[0], -b[2]});
        events.push_back({b[1], b[2]});
    }
    
    sort(events.begin(), events.end());
    vector<vector<int>> result;
    multiset<int> heights = {0};
    int prevMaxHeight = 0;
    
    for (auto& e : events) {
        int x = e.first;
        int h = e.second;
        
        if (h < 0) {
            heights.insert(-h);
        } else {
            heights.erase(heights.find(h));
        }
        
        int currentMaxHeight = *heights.rbegin();
        if (currentMaxHeight != prevMaxHeight) {
            result.push_back({x, currentMaxHeight});
            prevMaxHeight = currentMaxHeight;
        }
    }
    
    return result;
}
```

## New Keywords / STL Used
`std::multiset`, `rbegin()`

## Edge Cases
- Multiple buildings starting and ending at the same coordinates.
- Buildings of the same height.
