---
type: concept
tags: [queue, cpp, math, arrays, modular-arithmetic]
date: 2026-07-01
---
# Circular Queue (Ring Buffer)

## Problem Statement
Implement the Queue ADT using a fixed-capacity contiguous array $A$ of size $C$, eradicating the "Linear Drift" spatial exhaustion anomaly by mathematically establishing a cyclic topological ring.

---

## Approach: Modular Congruence 

Linear Drift occurs when both `front` and `rear` pointers monotonically increase, exhausting the spatial limit $C$ while previous lower indices remain functionally void.
We topologically warp the contiguous array into a geometric ring using **Modular Arithmetic** over the finite field $\mathbb{Z}_C$.

**State Variables:**
- `front`: Index of the absolute oldest element.
- `rear`: Index of the most recently inserted element.
- `size`: Absolute current element cardinality (resolves empty vs. full ambiguity).

**State Machine over $\mathbb{Z}_C$:**
1. **Initialization:** `front = 0`, `rear = -1`, `size = 0`.
2. **Enqueue($x$):**
   Constraint: `size < C`.
   Topology Update: $\text{rear} = (\text{rear} + 1) \pmod C$.
   Memory Map: $A[\text{rear}] = x$.
   `size++`.
3. **Dequeue():**
   Constraint: `size > 0`.
   Topology Update: $\text{front} = (\text{front} + 1) \pmod C$.
   `size--`.

The modular operator ensures that any pointer reaching index $C$ instantaneously wraps back to geometric origin $0$.

---

## Code Implementation

```cpp
#include <iostream>
#include <stdexcept>
#include <vector>

using namespace std;

class CircularQueue {
private:
    vector<int> arr;
    int front, rear, current_size, capacity;

public:
    CircularQueue(int k) : capacity(k), front(0), rear(-1), current_size(0) {
        arr.resize(capacity);
    }
    
    bool enqueue(int value) {
        if (isFull()) return false;
        
        // Cyclic modular step
        rear = (rear + 1) % capacity;
        arr[rear] = value;
        current_size++;
        return true;
    }
    
    bool dequeue() {
        if (isEmpty()) return false;
        
        // Cyclic modular step
        front = (front + 1) % capacity;
        current_size--;
        return true;
    }
    
    int Front() {
        if (isEmpty()) return -1;
        return arr[front];
    }
    
    int Rear() {
        if (isEmpty()) return -1;
        return arr[rear];
    }
    
    bool isEmpty() {
        return current_size == 0;
    }
    
    bool isFull() {
        return current_size == capacity;
    }
};
```

---

## Complexity Analysis
- **Time Complexity:** $O(1)$ strictly for all operations bounded by static modular arithmetic mapping.
- **Space Complexity:** $O(C)$ static pre-allocation.

> [!warning]
> **Ambiguity in $\mathbb{Z}_C$ without Size State:** If the `size` variable is omitted, the terminal topological state $\text{front} == (\text{rear} + 1) \pmod C$ becomes mathematically ambiguous—it can denote either an entirely empty queue or an entirely full queue! Maintaining `size` decouples this state anomaly.
