# Meeting Rooms

## Problem Statement
- given an array of meeting time intervals where $\text{intervals}[i] = [\text{start}_i, \text{end}_i]$, determine if a person could attend all meetings.

- *example:* $\text{intervals} = [[0,30],[5,10],[15,20]]$
- *result:* `false` (Overlaps exist)

- *example:* $\text{intervals} = [[7,10],[2,4]]$
- *result:* `true` (No overlaps)


## Approach: Sort by Start Time

- to check if there are any overlaps, we can conceptually align the intervals on a timeline. This is achieved via [[Sorting]].

- sort the intervals in strictly ascending order based on their starting times.
- iterate through the sorted intervals starting from the second one ($i = 1$).
- if the starting time of the current meeting $\text{intervals}[i][0]$ is strictly less than the ending time of the previous meeting $\text{intervals}[i-1][1]$, an overlap exists. The person cannot attend both. Return `false`.
- if no such overlaps are detected, return `true`.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

bool canAttendMeetings(vector<vector<int>>& intervals) {
    if (intervals.empty()) return true;
    
    // Sort intervals by their start times
    sort(intervals.begin(), intervals.end(), 
         [](const vector<int>& a, const vector<int>& b) {
             return a[0] < b[0];
         });
         
    // Check for overlaps
    for (size_t i = 1; i < intervals.size(); i++) {
        // If current meeting starts before the previous one ends
        if (intervals[i][0] < intervals[i - 1][1]) {
            return false;
        }
    }
    
    return true;
}
```

> [!warning]
> Meetings that end exactly when another begins (e.g., $[1, 2]$ and $[2, 3]$) are generally *not* considered overlapping in these standard problems. Hence, we use strict inequality `<` instead of `<=`.


## Complexity Analysis
- **time Complexity:** $O(N \log N)$. The time is dominated by the sorting step. The linear scan takes $O(N)$.
- **space Complexity:** $O(1)$ auxiliary space, not counting the stack space for sorting.

NEXT: [[Index]]
