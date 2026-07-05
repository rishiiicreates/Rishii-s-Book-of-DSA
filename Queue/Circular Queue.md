# Circular Queue (Ring Buffer)

## Problem Statement
- implement the Queue ADT using a fixed-capacity contiguous array $A$ of size $C$, eradicating the "Linear Drift" spatial exhaustion anomaly by mathematically establishing a cyclic topological ring.


## Approach: Modular Congruence 

- linear Drift occurs when both `front` and `rear` pointers monotonically increase, exhausting the spatial limit $C$ while previous lower indices remain functionally void.
- we topologically warp the contiguous array into a geometric ring using **Modular Arithmetic** over the finite field $\mathbb{Z}_C$.

- **state Variables:**
- `front`: Index of the absolute oldest element.
- `rear`: Index of the most recently inserted element.
- `size`: Absolute current element cardinality (resolves empty vs. full ambiguity).

- **state Machine over $\mathbb{Z}_C$:**
- **initialization:** `front = 0`, `rear = -1`, `size = 0`.
- **enqueue($x$):**
   - constraint: `size < C`.
   - topology Update: $\text{rear} = (\text{rear} + 1) \pmod C$.
   - memory Map: $A[\text{rear}] = x$.
   - `size++`.
- **dequeue():**
   - constraint: `size > 0`.
   - topology Update: $\text{front} = (\text{front} + 1) \pmod C$.
   - `size--`.

- the modular operator ensures that any pointer reaching index $C$ instantaneously wraps back to geometric origin $0$.


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


## Complexity Analysis
- **time Complexity:** $O(1)$ strictly for all operations bounded by static modular arithmetic mapping.
- **space Complexity:** $O(C)$ static pre-allocation.

> [!warning]
> **Ambiguity in $\mathbb{Z}_C$ without Size State:** If the `size` variable is omitted, the terminal topological state $\text{front} == (\text{rear} + 1) \pmod C$ becomes mathematically ambiguous—it can denote either an entirely empty queue or an entirely full queue! Maintaining `size` decouples this state anomaly.

NEXT: [[Index]]
