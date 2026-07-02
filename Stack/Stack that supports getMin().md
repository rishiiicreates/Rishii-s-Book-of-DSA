---
type: concept
tags: [stack, cpp]
date: 2026-06-30
---
# Stack that Supports getMin()

## Problem Statement
Design a stack that supports `push`, `pop`, `top`, and retrieving the minimum element in constant time, `O(1)`.

## Approach / Intuition
We can store a pair of `(value, current_minimum)` in a standard [[Stack]]. When pushing a new element, we compute the minimum between the new element and the previous minimum (available at the top of the stack). This uses extra space. Alternatively, for an O(1) space optimization, we can encode the values using a formula `2 * value - minElement`, which helps recover the previous minimum upon popping. We'll implement the pair approach for simplicity and robustness.

## Time & Space Complexity
- **[[Time Complexity]]:** O(1) for all operations
- **[[Space Complexity]]:** O(N) where N is the number of elements

## Sample Code
```cpp
#include <stack>
#include <algorithm>
using namespace std;

class MinStack {
    stack<pair<int, int>> st;
public:
    MinStack() {
    }
    
    void push(int val) {
        if (st.empty()) {
            st.push({val, val});
        } else {
            st.push({val, min(val, st.top().second)});
        }
    }
    
    void pop() {
        st.pop();
    }
    
    int top() {
        return st.top().first;
    }
    
    int getMin() {
        return st.top().second;
    }
};
```

## New Keywords / STL Used
`std::pair`, `std::stack`, `std::min`

## Edge Cases
Calling top or getMin on an empty stack (constraints usually guarantee valid calls), duplicate minimum values.
