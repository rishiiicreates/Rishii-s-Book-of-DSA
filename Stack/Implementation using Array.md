# Stack Implementation using Array

## Problem Statement
- implement the Stack ADT mathematically bounded by a fixed spatial capacity using contiguous memory (Array).


## Approach: Boundary Cursor (Top Pointer)

- we model the stack over a contiguous sequence block $A$ of fixed cardinality $C$.
- we define a boundary cursor `top_index`, initialized to $-1$ to represent the $\Lambda$ (empty) state.

- **state Transitions:**
- **push($x$):**
   - constraint: `top_index` $< C - 1$. (Overflow Guard).
   - state Update: `top_index` $\leftarrow$ `top_index` $+ 1$.
   - memory Map: $A[\text{top\_index}] \leftarrow x$.
- **pop():**
   - constraint: `top_index` $\ge 0$. (Underflow Guard).
   - state Update: `top_index` $\leftarrow$ `top_index` $- 1$.
   - (note: Memory at $A[\text{top\_index} + 1]$ is topologically discarded, but physically intact until overwritten).
- **top():**
   - constraint: `top_index` $\ge 0$.
   - return: $A[\text{top\_index}]$.


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


## Complexity Analysis
- **time Complexity:** $O(1)$ exact time for all operations. No amortization required as spatial bounds are strictly static.
- **space Complexity:** $O(C)$ strict pre-allocated memory, where $C$ is the static capacity, regardless of active element count.

> [!warning]
> **Static Bound Limitation:** The mathematical fatal flaw of the Array-based stack is its strict upper bound limit $C$. A dynamically resizing vector eliminates this, but shifts the Time Complexity of `push` from $O(1)$ exact to $O(1)$ amortized, as capacity duplication requires an $O(N)$ reallocation copy sequence.

NEXT: [[Index]]
