# K Queues in a Single Array

## Problem Statement
- design an optimal data structure to efficiently implement $K$ independent queues utilizing a strictly singular scalar array of size $N$. The structure must ensure that a `QueueOverflow` error is only triggered mathematically when all $N$ slots across the universal array are completely exhausted (i.e., no artificial fragmentation limitations).


## Approach: Spatial Linking and Free List Architecture

- a naive approach partitions the array into $K$ contiguous, static blocks of size $N/K$. However, this causes severe fragmentation: queue $i$ might overflow its bounds while queue $j$ is completely empty, squandering valid memory.

- to achieve optimal mathematical utilization, we dynamically manage memory using a topological approach identical to the OS file system's linked allocation.

### Necessary Arrays
- we require one primary data array and three auxiliary state-tracking arrays:
- `arr[N]`: Stores the actual mathematical values of the queue nodes.
- `front[K]`: Stores the physical index of the front element for each queue $i$. Initialize all to $-1$.
- `rear[K]`: Stores the physical index of the rear element for each queue $i$. Initialize all to $-1$.
- `next[N]`: Acts as a dual-purpose pointer map.
   - for an occupied slot $x$, `next[x]` stores the index of the next element in the *same* queue.
   - for a vacant slot $x$, `next[x]` stores the index of the next vacant slot, forming a unified **Free List**.

### Universal Variables
- `free_top`: Points to the mathematical head of the Free List. Initialized to $0$.

### Operations
- **enqueue(item, q_id):**
- ensure `free_top != -1` (otherwise, universal overflow).
- allocate index: `idx = free_top`. Update `free_top = next[idx]`.
- if queue $q_{id}$ is empty (`front[q_id] == -1`), set `front[q_id] = idx`.
- else, link the old rear: `next[rear[q_id]] = idx`.
- update state: `next[idx] = -1`, `rear[q_id] = idx`, `arr[idx] = item`.

- **dequeue(q_id):**
- ensure `front[q_id] != -1` (otherwise, queue underflow).
- retrieve target: `idx = front[q_id]`.
- advance front pointer: `front[q_id] = next[idx]`.
- deallocate and return to free list: `next[idx] = free_top`, `free_top = idx`.
- return `arr[idx]`.


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


## Complexity Analysis
- **time Complexity:** $O(1)$ for both `enqueue` and `dequeue`. Memory resolution via the `next` array guarantees strict constant-time boundaries.
- **space Complexity:** $O(N + K)$ overhead for the auxiliary tracking arrays, independent of the underlying values.

NEXT: [[Index]]
