---
type: concept
tags: [queue, stack, cpp, amortized-analysis]
date: 2026-07-01
---
# Queue Implementation using Stacks

## Problem Statement
Simulate a structurally robust First-In-First-Out (FIFO) Queue ADT using exclusively two standard Last-In-First-Out (LIFO) Stack data structures. 

---

## Approach: Temporal Inversion

A single stack imposes LIFO topology. To extract the "First-In" element (which is buried at the geometric bottom of the stack), we must geometrically invert the entire stack sequence. Passing elements from Stack A to Stack B exactly once reverses their temporal order.

We define two structural domains:
1. `Input Stack` ($S_i$): Absorbs all incoming `enqueue` requests in raw LIFO sequence.
2. `Output Stack` ($S_o$): Serves `dequeue` requests in pure FIFO sequence.

**State Machine (Lazy Inversion):**
- **Enqueue($x$):** Push $x$ onto $S_i$. (Cost: $O(1)$ exact).
- **Dequeue() / Front():** 
  If $S_o$ is populated, simply pop/top $S_o$. (Cost: $O(1)$ exact).
  If $S_o$ is strictly empty ($\Lambda$), a systemic topological inversion is mandatory. We atomically transfer every element from $S_i$ into $S_o$ via `pop` & `push`. The absolute bottom of $S_i$ maps to the absolute top of $S_o$, generating a mathematically perfect FIFO state. (Cost: $O(N)$ rare burst).

---

## Code Implementation

```cpp
#include <stack>
#include <stdexcept>

using namespace std;

class MyQueue {
private:
    stack<int> s_input;
    stack<int> s_output;

    // Temporal Inversion Subroutine
    void transferIfNeeded() {
        if (s_output.empty()) {
            while (!s_input.empty()) {
                s_output.push(s_input.top());
                s_input.pop();
            }
        }
    }

public:
    MyQueue() {}
    
    void push(int x) {
        s_input.push(x);
    }
    
    int pop() {
        transferIfNeeded();
        if (s_output.empty()) throw underflow_error("Queue Empty");
        
        int val = s_output.top();
        s_output.pop();
        return val;
    }
    
    int peek() {
        transferIfNeeded();
        if (s_output.empty()) throw underflow_error("Queue Empty");
        
        return s_output.top();
    }
    
    bool empty() {
        return s_input.empty() && s_output.empty();
    }
};
```

---

## Complexity Analysis
- **Time Complexity:** $O(1)$ **Amortized**. While a specific `pop()` may trigger an $O(N)$ inversion burst, every scalar is transferred between the two stacks exactly *once* during its absolute lifetime. Thus, $N$ operations cost $O(N)$ time, averaging strictly to $O(1)$ amortized per operation.
- **Space Complexity:** $O(N)$ composite auxiliary memory tracking bounded sequence elements.

> [!tip]
> **Amortization Guarantee:** The algorithm acts lazily. Do *not* eagerly dump elements back to $S_i$ after a pop. That naive implementation mathematically destroys the $O(1)$ amortized bound, devolving the structural integrity into strict $O(N^2)$ quadratic decay over cyclic pop operations.
