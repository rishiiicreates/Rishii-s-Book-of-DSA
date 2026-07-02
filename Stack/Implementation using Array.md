---
type: concept
tags: [stack, cpp, arrays, memory-management]
date: 2026-07-01
---
# Stack Implementation using Array

## Problem Statement
Implement the Stack ADT mathematically bounded by a fixed spatial capacity using contiguous memory (Array).

---

## Approach: Boundary Cursor (Top Pointer)

We model the stack over a contiguous sequence block $A$ of fixed cardinality $C$. 
We define a boundary cursor `top_index`, initialized to $-1$ to represent the $\Lambda$ (empty) state.

**State Transitions:**
1. **Push($x$):** 
   Constraint: `top_index` $< C - 1$. (Overflow Guard).
   State Update: `top_index` $\leftarrow$ `top_index` $+ 1$. 
   Memory Map: $A[\text{top\_index}] \leftarrow x$.
2. **Pop():**
   Constraint: `top_index` $\ge 0$. (Underflow Guard).
   State Update: `top_index` $\leftarrow$ `top_index` $- 1$.
   (Note: Memory at $A[\text{top\_index} + 1]$ is topologically discarded, but physically intact until overwritten).
3. **Top():**
   Constraint: `top_index` $\ge 0$.
   Return: $A[\text{top\_index}]$.

---

## Code Implementation

```cpp
#include <iostream>
#include <stdexcept>

using namespace std;

class ArrayStack {
private:
    int* arr;
    int top_index;
    int capacity;

public:
    // Constructor allocates strict contiguous memory block
    ArrayStack(int size) : capacity(size), top_index(-1) {
        arr = new int[capacity];
    }
    
    // Destructor reclaims memory mapping
    ~ArrayStack() {
        delete[] arr;
    }
    
    void push(int x) {
        if (top_index == capacity - 1) {
            throw overflow_error("Stack Overflow");
        }
        arr[++top_index] = x;
    }
    
    void pop() {
        if (top_index == -1) {
            throw underflow_error("Stack Underflow");
        }
        top_index--;
    }
    
    int top() const {
        if (top_index == -1) {
            throw underflow_error("Stack Underflow");
        }
        return arr[top_index];
    }
    
    bool empty() const {
        return top_index == -1;
    }
};
```

---

## Complexity Analysis
- **Time Complexity:** $O(1)$ exact time for all operations. No amortization required as spatial bounds are strictly static.
- **Space Complexity:** $O(C)$ strict pre-allocated memory, where $C$ is the static capacity, regardless of active element count.

> [!warning]
> **Static Bound Limitation:** The mathematical fatal flaw of the Array-based stack is its strict upper bound limit $C$. A dynamically resizing vector eliminates this, but shifts the Time Complexity of `push` from $O(1)$ exact to $O(1)$ amortized, as capacity duplication requires an $O(N)$ reallocation copy sequence.
