# Deque: Circular Array Implementation

## Problem Statement
- implement the Deque ADT using a fixed-capacity contiguous array $A$ of size $C$, maintaining strict $O(1)$ time complexity for all four boundary mutations (front/back).


## Approach: Bidirectional Modular Congruence 

- to permit topological expansion in both the negative and positive directions simultaneously without spatial exhaustion (Linear Drift), we project the linear array geometry into a cyclic ring using **Modular Arithmetic** over the finite field $\mathbb{Z}_C$.

- **state Variables:**
- `front`: Topologically bounds the first valid element.
- `rear`: Topologically bounds the final valid element.
- `size`: Absolute current element cardinality (resolves empty/full paradox).

- **state Machine over $\mathbb{Z}_C$:**
- **push Back ($x$):**
   - topology Update: $\text{rear} = (\text{rear} + 1) \pmod C$.
   - memory Map: $A[\text{rear}] = x$.
- **pop Back ():**
   - topology Update: $\text{rear} = (\text{rear} - 1 + C) \pmod C$. (The $+C$ guarantees a strictly positive modulus).
- **push Front ($x$):**
   - topology Update: $\text{front} = (\text{front} - 1 + C) \pmod C$. (Warping into the negative domain).
   - memory Map: $A[\text{front}] = x$.
- **pop Front ():**
   - topology Update: $\text{front} = (\text{front} + 1) \pmod C$.


## Code Implementation

```cpp
#include <iostream>
#include <stdexcept>
#include <vector>

using namespace std;

class CircularDeque {
private:
    vector<int> arr;
    int front, rear, current_size, capacity;

public:
    CircularDeque(int k) : capacity(k), front(0), rear(-1), current_size(0) {
        arr.resize(capacity);
    }
    
    bool insertFront(int value) {
        if (isFull()) return false;
        // Negative Domain modular transition
        front = (front - 1 + capacity) % capacity;
        arr[front] = value;
        // Re-anchor rear if this is the first absolute element
        if (current_size == 0) rear = front;
        current_size++;
        return true;
    }
    
    bool insertLast(int value) {
        if (isFull()) return false;
        // Positive Domain modular transition
        rear = (rear + 1) % capacity;
        arr[rear] = value;
        // Re-anchor front if this is the first absolute element
        if (current_size == 0) front = rear;
        current_size++;
        return true;
    }
    
    bool deleteFront() {
        if (isEmpty()) return false;
        front = (front + 1) % capacity;
        current_size--;
        return true;
    }
    
    bool deleteLast() {
        if (isEmpty()) return false;
        rear = (rear - 1 + capacity) % capacity;
        current_size--;
        return true;
    }
    
    int getFront() {
        if (isEmpty()) return -1;
        return arr[front];
    }
    
    int getRear() {
        if (isEmpty()) return -1;
        return arr[rear];
    }
    
    bool isEmpty() { return current_size == 0; }
    bool isFull() { return current_size == capacity; }
};
```


## Complexity Analysis
- **time Complexity:** $O(1)$ strictly for all operations. The modulus bounds prevent iterative scans.
- **space Complexity:** $O(C)$ static pre-allocation.

> [!tip]
> **The Negative Modulo Trap:** In C++, the `%` operator evaluates the remainder, not the strict mathematical modulo. Thus, `-1 % 5` resolves to `-1` structurally, generating out-of-bounds memory faults. The geometric correction $(X - 1 + C) \pmod C$ forces the domain into purely positive scalar values, strictly preserving ring topology.

NEXT: [[Index]]
