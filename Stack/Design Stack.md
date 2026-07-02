---
type: concept
tags: [stack, cpp, math, design-pattern]
date: 2026-07-01
---
# Design Min Stack

## Problem Statement
Formulate a strictly augmented Stack ADT that perfectly preserves $O(1)$ LIFO primitive operations, whilst concurrently establishing an exogenous `getMin()` operation retrieving the absolute minimum mathematical scalar resident in the stack in strictly $O(1)$ time.

---

## Approach: Dual-Stack Isomorphic Shadowing

A naive search for the minimum scalar evaluates to $O(N)$. To achieve $O(1)$, we mathematically cache the state of the minimum variable at *every single topological depth*.

We construct two synchronously bound sequence vectors:
1. `Main Stack`: Stores the absolute input scalars sequentially.
2. `Min Stack`: Stores the mathematical prefix minimum bound up to the corresponding depth of the main stack.

**State Machine Symmetry:**
- **Push($x$):** 
  Main Stack pushes $x$.
  Min Stack computes the absolute mathematical bound: $M = \min(x, \text{Min Stack.top()})$.
  Min Stack pushes $M$.
- **Pop():**
  Both structures issue a strictly synchronized `pop()`, instantly reverting the historical state of the system bounds.

---

## Code Implementation

```cpp
#include <stack>
#include <algorithm>
#include <stdexcept>

using namespace std;

class MinStack {
private:
    stack<int> main_stack;
    stack<int> min_stack;

public:
    MinStack() {}
    
    void push(int val) {
        main_stack.push(val);
        // Compute recursive topological bound
        if (min_stack.empty()) {
            min_stack.push(val);
        } else {
            min_stack.push(min(val, min_stack.top()));
        }
    }
    
    void pop() {
        if (main_stack.empty()) throw underflow_error("Stack Underflow");
        main_stack.pop();
        min_stack.pop(); // Revert bound history synchronously
    }
    
    int top() {
        if (main_stack.empty()) throw underflow_error("Stack Underflow");
        return main_stack.top();
    }
    
    int getMin() {
        if (min_stack.empty()) throw underflow_error("Stack Underflow");
        return min_stack.top();
    }
};
```

---

## Complexity Analysis
- **Time Complexity:** $O(1)$ amortized for `push`, `pop`, `top`, and strictly $O(1)$ for `getMin`.
- **Space Complexity:** $O(N)$ for the primary scalar tracking + $O(N)$ for the bound shadow stack. Overall $O(N)$ auxiliary space.

> [!tip]
> **Math Encoding Optimization ($O(1)$ Space):** The $O(N)$ shadow stack can be eradicated by mathematically encoding the difference $\Delta = x - \text{current\_min}$ into the main stack. If $\Delta < 0$, it implies a new absolute minimum, and we push $\Delta$ while overwriting `current_min`. Upon popping a negative $\Delta$, we compute the historical bound mathematically via $\text{prev\_min} = \text{current\_min} - \Delta$. This reduces space to strict $O(1)$ overhead but requires `long long` limits to survive $\Delta$ integer overflow.
