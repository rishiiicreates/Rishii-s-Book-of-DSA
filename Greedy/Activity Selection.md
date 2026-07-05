# Activity Selection

## Problem Statement
- given N activities with their start and finish times, select the maximum number of activities that can be performed by a single person, assuming only one activity can be worked on at a time.

## Approach / Intuition
- sort the activities based on their finish times in ascending order. Pick the first activity and then iterate through the rest. If an activity starts after or at the same time the previously selected activity finishes, select it. This [[Greedy Algorithm]] ensures maximum activities are chosen.

## Time & Space Complexity
- **[[time Complexity]]:** O(N log N)
- **[[space Complexity]]:** O(N) for storing pairs

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

struct Activity {
    int start, finish;
};

bool cmp(Activity a, Activity b) {
    return a.finish < b.finish;
}

int maxActivities(vector<Activity>& activities) {
    sort(activities.begin(), activities.end(), cmp);
    int count = 1;
    int lastFinishTime = activities[0].finish;
    for (int i = 1; i < activities.size(); i++) {
        if (activities[i].start >= lastFinishTime) {
            count++;
            lastFinishTime = activities[i].finish;
        }
    }
    return count;
}
```

## New Keywords / STL Used
- custom comparator, struct

## Edge Cases
- all activities overlap
- no activities overlap

NEXT: [[Index]]
