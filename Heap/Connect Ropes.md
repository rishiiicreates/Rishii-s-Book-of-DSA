---
type: concept
tags: [heap, cpp, greedy, huffman]
date: 2026-06-30
---
# Connect Ropes

## Problem Statement
Given an array explicitly representing distinct lengths of disjoint ropes, securely connect them into one massive singular rope. The overall cost of connecting two physical ropes heavily equals their immediate sum. Minimize total sequential cost drastically.

## Approach / Intuition
At absolutely every distinct operational step, explicitly isolate and pick the two physically shortest available ropes. Calculate and add their aggregated combinatorial sum cleanly back into the active pool. Employing a strict Min Heap essentially ensures the absolute shortest lengths continually mathematically surface instantly, proving an unbreakable [[Greedy Strategy]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log N)
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
long long minCostRopes(vector<long long>& arr) {
    priority_queue<long long, vector<long long>, greater<long long>> pq(arr.begin(), arr.end());
    long long totalCost = 0;
    while (pq.size() > 1) {
        long long first = pq.top();
        pq.pop();
        long long second = pq.top();
        pq.pop();
        long long sum = first + second;
        totalCost += sum;
        pq.push(sum);
    }
    return totalCost;
}
```

## New Keywords / STL Used
Iterator-based initialization for priority_queue

## Edge Cases
Only exactly one single rope provided initially (cost evaluates 0 natively), extreme rope lengths effectively causing variable overflow.
