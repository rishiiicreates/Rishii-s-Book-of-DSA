---
type: concept
tags: [greedy, cpp, job-sequencing]
date: 2026-06-30
---
# Job Sequencing

## Problem Statement
Given a set of N jobs where each job comes with a deadline and profit, find the maximum profit that can be earned assuming each job takes one unit of time and can only be completed on or before its deadline.

## Approach / Intuition
Sort the jobs in descending order of profit. Iterate through the jobs and assign each to the latest possible available time slot on or before its deadline. This [[Greedy Algorithm]] ensures maximum profit by prioritizing high-profit jobs.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log N + N * max_deadline)
- **[[Space Complexity]]:** O(max_deadline)

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

struct Job {
    int id, dead, profit;
};

bool cmp(Job a, Job b) {
    return a.profit > b.profit;
}

vector<int> JobScheduling(Job arr[], int n) {
    sort(arr, arr + n, cmp);
    int maxi = arr[0].dead;
    for (int i = 1; i < n; i++) {
        maxi = max(maxi, arr[i].dead);
    }
    vector<int> slot(maxi + 1, -1);
    int countJobs = 0, jobProfit = 0;
    for (int i = 0; i < n; i++) {
        for (int j = arr[i].dead; j > 0; j--) {
            if (slot[j] == -1) {
                slot[j] = i;
                countJobs++;
                jobProfit += arr[i].profit;
                break;
            }
        }
    }
    return {countJobs, jobProfit};
}
```

## New Keywords / STL Used
- `struct` array, `vector` initialization with size

## Edge Cases
- All jobs have the same deadline
- Many jobs with high profits but small deadlines
