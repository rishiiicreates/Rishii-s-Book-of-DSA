# Merge Overlapping Intervals

## Problem Statement
- given a mathematical set of intervals $I = \{[s_0, e_0], [s_1, e_1], \dots, [s_{N-1}, e_{N-1}]\}$, merge all overlapping intervals and return an array of the non-overlapping intervals that cover exactly the same mathematical range.
- two intervals $[a, b]$ and $[c, d]$ overlap if and only if $a \le d$ and $c \le b$.


## Approach: Lexicographical Sort & Greedy Collapse

- if intervals are randomly distributed, resolving overlaps is computationally chaotic. We impose geometric order by sorting the intervals by their start times: $s_0 \le s_1 \le \dots \le s_{N-1}$.

- once sorted, a mathematical guarantee emerges: an interval $I_k = [s_k, e_k]$ can **only** overlap with intervals that appear immediately before or after it in the sorted sequence.

- we can apply a Greedy state-collapse algorithm:
- initialize an output array `merged` and push the first interval $I_0$.
- for each subsequent interval $I_k$:
   - let the most recently committed interval in `merged` be $M = [s_m, e_m]$.
   - because of our sort, we already know $s_m \le s_k$. The only condition for an overlap is if the new start time $s_k$ is bounded by the previous end time: $s_k \le e_m$.
   - **overlap Case:** If $s_k \le e_m$, the intervals merge. The new extended bound is the maximum of the two end times: $e_m = \max(e_m, e_k)$.
   - **disjoint Case:** If $s_k > e_m$, the intervals are completely disjoint. We push $I_k$ directly into `merged` as a new independent state.


## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

vector<vector<int>> merge(vector<vector<int>>& intervals) {
    if (intervals.empty()) return {};
    
    // Sorts intervals automatically by the 0th index (start time)
    sort(intervals.begin(), intervals.end());
    
    vector<vector<int>> merged;
    merged.push_back(intervals[0]); // Commit the initial state
    
    for (int i = 1; i < intervals.size(); i++) {
        // merged.back()[1] is e_m, intervals[i][0] is s_k
        if (merged.back()[1] >= intervals[i][0]) {
            // Overlap detected: Absorb the interval
            merged.back()[1] = max(merged.back()[1], intervals[i][1]);
        } else {
            // Disjoint: Create a new independent state
            merged.push_back(intervals[i]);
        }
    }
    
    return merged;
}
```


## Complexity Analysis
- **time Complexity:** $O(N \log N)$ strictly bounded by the `std::sort`. The subsequent linear greedy collapse takes exactly $O(N)$.
- **space Complexity:** $O(N)$ auxiliary space for the `merged` vector holding the resulting intervals.

> [!important]
> **Sub-interval Absorption:** Note that $e_m = \max(e_m, e_k)$ handles the specific case where $I_k$ is entirely enveloped within $M$ (e.g., merging $[1, 10]$ and $[2, 5]$ results in $[1, 10]$, not $[1, 5]$).

NEXT: [[Index]]
