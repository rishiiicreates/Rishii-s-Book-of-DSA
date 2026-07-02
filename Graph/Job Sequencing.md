---
type: concept
tags: [graph, cpp, greedy, disjoint-set]
date: 2026-06-30
---
# Job Sequencing

## Problem Statement
Given a set of jobs, each with a deadline and profit, maximize total profit assuming each job takes 1 unit of time.

## Approach / Intuition
Use a [[Greedy Algorithm]] combined with a [[Disjoint Set]] to efficiently find the latest available time slot for a job before its deadline. Sort jobs by descending profit and assign them.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N \log N)
- **[[Space Complexity]]:** O(M) where M is maximum deadline

## Sample Code
```cpp
class DisjointSet {
    vector<int> parent;
public:
    DisjointSet(int n) {
        parent.resize(n + 1);
        for (int i = 0; i <= n; i++) parent[i] = i;
    }
    int findUPar(int node) {
        if (node == parent[node]) return node;
        return parent[node] = findUPar(parent[node]);
    }
    void unionByRank(int u, int v) {
        parent[u] = v;
    }
};
struct Job {
    int id, dead, profit;
};
bool comp(Job a, Job b) {
    return a.profit > b.profit;
}
vector<int> JobScheduling(Job arr[], int n) {
    sort(arr, arr + n, comp);
    int maxDeadline = 0;
    for (int i = 0; i < n; i++) {
        maxDeadline = max(maxDeadline, arr[i].dead);
    }
    DisjointSet ds(maxDeadline);
    int countJobs = 0, jobProfit = 0;
    for (int i = 0; i < n; i++) {
        int availableSlot = ds.findUPar(arr[i].dead);
        if (availableSlot > 0) {
            countJobs++;
            jobProfit += arr[i].profit;
            ds.unionByRank(availableSlot, ds.findUPar(availableSlot - 1));
        }
    }
    return {countJobs, jobProfit};
}
```

## New Keywords / STL Used
`struct`, `sort`, custom comparator

## Edge Cases
All jobs have same deadline, deadlines are earlier than the number of jobs, empty job array.
