---
type: concept
tags: [stack, cpp]
date: 2026-06-30
---
# Implement Stack using Queues

## Problem Statement
Implement a LIFO stack using only two queues (or one queue). The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

## Approach / Intuition
A [[Queue]] is FIFO, while a [[Stack]] is LIFO. We can achieve stack behavior using a single queue. On every `push` operation, we append the new element to the back of the queue, then we dequeue all previous elements and re-enqueue them one by one. This process reverses the order, effectively bringing the newly inserted element to the front of the queue.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) for push, O(1) for pop, top, and empty
- **[[Space Complexity]]:** O(N) for storing elements in the queue

## Sample Code
```cpp
#include <queue>
using namespace std;

class MyStack {
    queue<int> q;
public:
    MyStack() {
    }
    
    void push(int x) {
        q.push(x);
        int sz = q.size();
        for (int i = 0; i < sz - 1; i++) {
            q.push(q.front());
            q.pop();
        }
    }
    
    int pop() {
        int val = q.front();
        q.pop();
        return val;
    }
    
    int top() {
        return q.front();
    }
    
    bool empty() {
        return q.empty();
    }
};
```

## New Keywords / STL Used
`std::queue`, `class`

## Edge Cases
Calling pop or top on an empty stack (can assume valid calls per standard problem description).
