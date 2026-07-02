---
type: concept
tags: [greedy, cpp, min-time-finish-jobs]
date: 2026-06-30
---
# Min Time to Finish All Jobs with Given Constraints

## Problem Statement
Given an array of jobs with their times and `K` identical assignees. An assignee can do continuous jobs. Find the minimum time to finish all jobs. The time taken is the maximum time among all assignees.

## Approach / Intuition
Use [[Binary Search]] for the answer. The search space is between the max job time and the sum of all job times. For a mid value, use a [[Greedy Algorithm]] to check if all jobs can be distributed to `K` assignees such that no assignee takes more than `mid` time.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log(Sum - Max))
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
#include <vector>
#include <numeric>
#include <algorithm>

using namespace std;

bool isPossible(vector<int>& jobs, int k, int maxTime) {
    int currentAssigneeTime = 0;
    int assigneesReq = 1;
    for (int i = 0; i < jobs.size(); i++) {
        if (currentAssigneeTime + jobs[i] > maxTime) {
            assigneesReq++;
            currentAssigneeTime = jobs[i];
            if (assigneesReq > k) return false;
        } else {
            currentAssigneeTime += jobs[i];
        }
    }
    return true;
}

int minTime(vector<int>& jobs, int k) {
    int low = *max_element(jobs.begin(), jobs.end());
    int high = accumulate(jobs.begin(), jobs.end(), 0);
    int ans = high;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (isPossible(jobs, k, mid)) {
            ans = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    return ans;
}
```

## New Keywords / STL Used
- `max_element`, `accumulate`

## Edge Cases
- Single assignee
- Single job
