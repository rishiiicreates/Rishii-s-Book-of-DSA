---
type: concept
tags: [queue, deque, cpp, data-structure]
date: 2026-06-30
---
# Queue

## Problem Statement
A linear data structure that follows the First-In-First-Out (FIFO) principle, where elements are inserted at the rear and removed from the front.

## Approach / Intuition
The standard [[Queue]] models real-world lines. Operations include `push` (enqueue), `pop` (dequeue), `front`, and `back`. It is primarily used for breadth-first [[Graph Traversal]] and scheduling scenarios.

## Time & Space Complexity
- **[[Time Complexity]]:** O(1) for push, pop, front, back
- **[[Space Complexity]]:** O(n)

## Sample Code
```cpp
void queueOperations() {
    queue<int> q;
    q.push(10);
    q.push(20);
    int first = q.front();
    q.pop();
}
```

## New Keywords / STL Used
`std::queue`, `push`, `pop`, `front`

## Edge Cases
Popping from an empty queue, accessing front/back of an empty queue.
