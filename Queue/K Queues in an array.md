---
type: concept
tags: [queue, array, memory-management, cpp]
date: 2026-06-30
---
# K Queues in a Single Array

## Problem Statement
Design an optimal data structure to efficiently implement $K$ independent queues utilizing a strictly singular scalar array of size $N$. The structure must ensure that a `QueueOverflow` error is only triggered mathematically when all $N$ slots across the universal array are completely exhausted (i.e., no artificial fragmentation limitations).

---

## Approach: Spatial Linking and Free List Architecture

A naive approach partitions the array into $K$ contiguous, static blocks of size $N/K$. However, this causes severe fragmentation: queue $i$ might overflow its bounds while queue $j$ is completely empty, squandering valid memory. 

To achieve optimal mathematical utilization, we dynamically manage memory using a topological approach identical to the OS file system's linked allocation.

### Necessary Arrays
We require one primary data array and three auxiliary state-tracking arrays:
1. `arr[N]`: Stores the actual mathematical values of the queue nodes.
2. `front[K]`: Stores the physical index of the front element for each queue $i$. Initialize all to $-1$.
3. `rear[K]`: Stores the physical index of the rear element for each queue $i$. Initialize all to $-1$.
4. `next[N]`: Acts as a dual-purpose pointer map.
   - For an occupied slot $x$, `next[x]` stores the index of the next element in the *same* queue.
   - For a vacant slot $x$, `next[x]` stores the index of the next vacant slot, forming a unified **Free List**.

### Universal Variables
- `free_top`: Points to the mathematical head of the Free List. Initialized to $0$.

### Operations
**Enqueue(item, q_id):**
1. Ensure `free_top != -1` (otherwise, universal overflow).
2. Allocate index: `idx = free_top`. Update `free_top = next[idx]`.
3. If queue $q_{id}$ is empty (`front[q_id] == -1`), set `front[q_id] = idx`.
4. Else, link the old rear: `next[rear[q_id]] = idx`.
5. Update state: `next[idx] = -1`, `rear[q_id] = idx`, `arr[idx] = item`.

**Dequeue(q_id):**
1. Ensure `front[q_id] != -1` (otherwise, queue underflow).
2. Retrieve target: `idx = front[q_id]`.
3. Advance front pointer: `front[q_id] = next[idx]`.
4. Deallocate and return to free list: `next[idx] = free_top`, `free_top = idx`.
5. Return `arr[idx]`.

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>
using namespace std;

class KQueues {
private:
    int* arr;
    int* front;
    int* rear;
    int* next;
    int n, k;
    int free_top;

public:
    KQueues(int k1, int n1) {
        k = k1;
        n = n1;
        arr = new int[n];
        next = new int[n];
        front = new int[k];
        rear = new int[k];
        
        free_top = 0;
        
        for (int i = 0; i < k; ++i) {
            front[i] = -1;
            rear[i] = -1;
        }
        
        for (int i = 0; i < n - 1; ++i) {
            next[i] = i + 1;
        }
        next[n - 1] = -1;
    }
    
    ~KQueues() {
        delete[] arr;
        delete[] next;
        delete[] front;
        delete[] rear;
    }
    
    void enqueue(int item, int q_id) {
        if (free_top == -1) {
            cout << "Queue Overflow\n";
            return;
        }
        
        int idx = free_top;
        free_top = next[idx];
        
        if (front[q_id] == -1) {
            front[q_id] = idx;
        } else {
            next[rear[q_id]] = idx;
        }
        
        next[idx] = -1;
        rear[q_id] = idx;
        arr[idx] = item;
    }
    
    int dequeue(int q_id) {
        if (front[q_id] == -1) {
            cout << "Queue Underflow\n";
            return -1;
        }
        
        int idx = front[q_id];
        front[q_id] = next[idx];
        
        next[idx] = free_top;
        free_top = idx;
        
        return arr[idx];
    }
};
```

> [!tip]
> This architecture relies heavily on array-based pointers. It achieves absolute optimization of cache-locality compared to an array of $K$ distinct Linked Lists distributed across the heap, minimizing cache-miss penalties.

---

## Complexity Analysis
- **Time Complexity:** $O(1)$ for both `enqueue` and `dequeue`. Memory resolution via the `next` array guarantees strict constant-time boundaries.
- **Space Complexity:** $O(N + K)$ overhead for the auxiliary tracking arrays, independent of the underlying values.
