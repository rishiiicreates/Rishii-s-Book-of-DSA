# Implement Stack using Queues

## Problem Statement
- implement a last-in-first-out (LIFO) stack using only a single queue.

## Approach / Intuition
- to simulate a [[Stack]], every time an element is pushed into the queue, rotate the queue. That means, push the new element, then pop and push all the existing elements `size() - 1` times so that the newly added element always stays at the front of the [[Queue]].

## Time & Space Complexity
- **[[time Complexity]]:** O(n) for push, O(1) for pop and top
- **[[space Complexity]]:** O(n)

## Sample Code
```cpp
class MyStack {
    queue<int> q;
public:
    void push(int x) {
        q.push(x);
        int sz = q.size();
        for (int i = 0; i < sz - 1; ++i) {
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
- `std::queue`

## Edge Cases
- popping/topping from an empty stack.

NEXT: [[Index]]
