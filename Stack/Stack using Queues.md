# Implement Stack using Queues

## Problem Statement
- implement a LIFO stack using only two queues (or one queue). The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

## Approach / Intuition
- a [[Queue]] is FIFO, while a [[Stack]] is LIFO. We can achieve stack behavior using a single queue. On every `push` operation, we append the new element to the back of the queue, then we dequeue all previous elements and re-enqueue them one by one. This process reverses the order, effectively bringing the newly inserted element to the front of the queue.

## Time & Space Complexity
- **[[time Complexity]]:** O(N) for push, O(1) for pop, top, and empty
- **[[space Complexity]]:** O(N) for storing elements in the queue

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
- `std::queue`, `class`

## Edge Cases
- calling pop or top on an empty stack (can assume valid calls per standard problem description).

NEXT: [[Index]]
