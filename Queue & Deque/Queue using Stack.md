---
type: concept
tags: [queue, deque, cpp, stack]
date: 2026-06-30
---
# Implement Queue using Stacks

## Problem Statement
Implement a first-in-first-out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue.

## Approach / Intuition
Use one [[Stack]] for pushing (`input`) and another for popping/peeking (`output`). Push operations go directly into `input`. For pop or peek, if `output` is empty, transfer all elements from `input` to `output`, effectively reversing their order to simulate FIFO behavior.

## Time & Space Complexity
- **[[Time Complexity]]:** O(1) amortized for push and pop
- **[[Space Complexity]]:** O(n)

## Sample Code
```cpp
class MyQueue {
    stack<int> input, output;
public:
    void push(int x) {
        input.push(x);
    }
    
    int pop() {
        peek();
        int val = output.top();
        output.pop();
        return val;
    }
    
    int peek() {
        if (output.empty()) {
            while (!input.empty()) {
                output.push(input.top());
                input.pop();
            }
        }
        return output.top();
    }
    
    bool empty() {
        return input.empty() && output.empty();
    }
};
```

## New Keywords / STL Used
`std::stack`

## Edge Cases
Popping/peeking when both stacks are empty, interleaved pushes and pops.
