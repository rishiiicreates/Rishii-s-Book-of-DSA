---
type: concept
tags: [queue, deque, cpp, data-structure]
date: 2026-06-30
---
# Deque (Double Ended Queue)

## Problem Statement
A generalized queue that allows insertion and deletion at both ends (front and rear).

## Approach / Intuition
A [[Deque]] combines the capabilities of both a stack and a queue. It is often implemented as a dynamic array of fixed-size arrays or a doubly linked list, enabling O(1) operations at both boundaries. It's heavily used in sliding window problems.

## Time & Space Complexity
- **[[Time Complexity]]:** O(1) for push/pop at both ends
- **[[Space Complexity]]:** O(n)

## Sample Code
```cpp
void dequeOperations() {
    deque<int> dq;
    dq.push_back(10);
    dq.push_front(20);
    dq.pop_back();
    dq.pop_front();
}
```

## New Keywords / STL Used
`std::deque`, `push_front`, `push_back`, `pop_front`, `pop_back`

## Edge Cases
Operations on an empty deque, iterators invalidation on resize.
