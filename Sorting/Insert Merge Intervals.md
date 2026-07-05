# Insert Merge Intervals

## Problem Statement
- given a set of non-overlapping intervals sorted by their start times, insert a new interval into the list. If the new interval overlaps with existing intervals, merge them appropriately. The resulting list must remain sorted and non-overlapping.

- *example:* $\text{intervals} = [[1,3],[6,9]]$, $\text{newInterval} = [2,5]$
- *result:* $[[1,5],[6,9]]$


## Approach: Linear Traversal & Greedy Merge

- since the intervals are already sorted by start time, we don't need to append and re-sort ($O(N \log N)$). We can accomplish this in $O(N)$ with a single pass.

- **left Non-Overlapping:** Iterate and push all intervals that end *strictly before* the `newInterval` starts into the result array.
- **merge Overlapping:** For all intervals that overlap with `newInterval`, merge them by mutating `newInterval`.
   - an interval overlaps if its start time $\le$ `newInterval[1]`.
   - update `newInterval` to $\left[ \min(\text{start}), \max(\text{end}) \right]$.
   - push the fully merged `newInterval` to the result.
- **right Non-Overlapping:** Push all remaining intervals (which start strictly after the merged `newInterval` ends) into the result.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

vector<vector<int>> insert(const vector<vector<int>>& intervals, vector<int>& newInterval) {
    vector<vector<int>> result;
    int i = 0;
    int n = intervals.size();
    
    // 1. Add all intervals ending before newInterval starts
    while (i < n && intervals[i][1] < newInterval[0]) {
        result.push_back(intervals[i]);
        i++;
    }
    
    // 2. Merge all overlapping intervals
    while (i < n && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = min(newInterval[0], intervals[i][0]);
        newInterval[1] = max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.push_back(newInterval); // Push the mathematically merged boundary
    
    // 3. Add all remaining intervals
    while (i < n) {
        result.push_back(intervals[i]);
        i++;
    }
    
    return result;
}
```

> [!tip]
> This pattern elegantly avoids modifying the original array in place (which would be $O(N^2)$ due to erasing/shifting elements in a `std::vector`). Generating a new result array ensures strict $O(N)$ runtime.


## Complexity Analysis
- **time Complexity:** $O(N)$. We make a single pass over the $N$ intervals.
- **space Complexity:** $O(N)$. The result array requires linear space to store the merged output.

NEXT: [[Index]]
