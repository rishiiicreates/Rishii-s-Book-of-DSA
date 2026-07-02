---
type: concept
tags: [heap, cpp, priority-queue]
date: 2026-06-30
---
# Priority Queue

## Problem Statement
Utilize a priority queue to maintain a highly dynamic stream of integers and retrieve the minimum or maximum continuously.

## Approach / Intuition
A `priority_queue` in C++ actively mimics a Max Heap natively by default. To emulate a Min Heap seamlessly, pass `std::greater<int>` strictly as the internal comparator. This container adapter fundamentally abstracts away raw array manipulation, yielding flawless [[Dynamic Queuing]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(log N) for insertion/deletion, O(1) for top
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
void demonstratePriorityQueue() {
    priority_queue<int> maxPQ;
    maxPQ.push(10);
    maxPQ.push(20);
    int topMax = maxPQ.top();
    maxPQ.pop();
    
    priority_queue<int, vector<int>, greater<int>> minPQ;
    minPQ.push(10);
    minPQ.push(20);
    int topMin = minPQ.top();
    minPQ.pop();
}
```

## New Keywords / STL Used
`std::priority_queue`, `std::greater`

## Edge Cases
Extracting objects directly from freshly initialized empty queues.
