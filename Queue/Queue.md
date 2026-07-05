# Queue Theory & Axiomatization

## Problem Statement
- formally define and axiomatize the Queue abstract data type (ADT) enforcing the strict First-In-First-Out (FIFO) topological ordering.


## Approach: Axiomatic Semantics (FIFO)

- a Queue $Q$ over a domain of elements $E$ is mathematically defined as a topologically directed sequence where structural mutations occur strictly at disjoint boundaries: insertions at the `Rear` (or Tail) and deletions at the `Front` (or Head).
- this enforces the temporal invariant: the element resident in the sequence for the absolute longest duration is the first to be extracted.

- **axiomatic Definition:**
- let $Q$ be a queue state, $x \in E$, and $\Lambda$ denote the empty queue.
- we define the primitive operations:
- $\text{enqueue}(Q, x) \to Q'$
- $\text{dequeue}(Q) \to Q''$
- $\text{front}(Q) \to x$
- $\text{empty}(Q) \to \text{Boolean}$

- **algebraic Axioms:**
- $\text{empty}(\Lambda) = \text{True}$
- $\text{empty}(\text{enqueue}(Q, x)) = \text{False}$
- $\text{front}(\text{enqueue}(\Lambda, x)) = x$
- $\text{front}(\text{enqueue}(Q, x)) = \text{front}(Q) \iff Q \ne \Lambda$
- $\text{dequeue}(\text{enqueue}(\Lambda, x)) = \Lambda$
- $\text{dequeue}(\text{enqueue}(Q, x)) = \text{enqueue}(\text{dequeue}(Q), x) \iff Q \ne \Lambda$
- $\text{dequeue}(\Lambda) = \text{Undefined (Underflow)}$

- any structural implementation must preserve these mapping axioms natively in $O(1)$ time bounds.


## Standard Template Library (STL) Implementation

```cpp
#include <iostream>
#include <queue>

using namespace std;

void stlQueueExample() {
    // std::queue is a container adaptor wrapping std::deque by default
    queue<int> q;
    
    q.push(10); // Q = [10] (Rear)
    q.push(20); // Q = [10, 20] (Rear)
    
    int front_val = q.front(); // Returns 10
    int back_val = q.back();   // Returns 20
    
    q.pop(); // Extracts 10. Q = [20]
    
    bool is_empty = q.empty(); // Returns false
}
```


## Complexity Analysis
- **time Complexity:** $O(1)$ amortized for all primitive sequence operations.
- **space Complexity:** $O(N)$ bound strictly to sequence cardinality.

> [!important]
> **Array Constraint:** Unlike a Stack, a linear array inherently fails the Queue constraints. If elements are enqueued at index $N$ and dequeued at index $0$, the structural array undergoes "Linear Drift", exhausting capacity despite internal fragmentation. This necessitates Circular buffer mathematics or Linked Node topology.

NEXT: [[Index]]
