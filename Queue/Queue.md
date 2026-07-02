---
type: concept
tags: [queue, cpp, math, data-structures]
date: 2026-07-01
---
# Queue Theory & Axiomatization

## Problem Statement
Formally define and axiomatize the Queue abstract data type (ADT) enforcing the strict First-In-First-Out (FIFO) topological ordering.

---

## Approach: Axiomatic Semantics (FIFO)

A Queue $Q$ over a domain of elements $E$ is mathematically defined as a topologically directed sequence where structural mutations occur strictly at disjoint boundaries: insertions at the `Rear` (or Tail) and deletions at the `Front` (or Head).
This enforces the temporal invariant: the element resident in the sequence for the absolute longest duration is the first to be extracted.

**Axiomatic Definition:**
Let $Q$ be a queue state, $x \in E$, and $\Lambda$ denote the empty queue.
We define the primitive operations:
1. $\text{enqueue}(Q, x) \to Q'$
2. $\text{dequeue}(Q) \to Q''$
3. $\text{front}(Q) \to x$
4. $\text{empty}(Q) \to \text{Boolean}$

**Algebraic Axioms:**
1. $\text{empty}(\Lambda) = \text{True}$
2. $\text{empty}(\text{enqueue}(Q, x)) = \text{False}$
3. $\text{front}(\text{enqueue}(\Lambda, x)) = x$
4. $\text{front}(\text{enqueue}(Q, x)) = \text{front}(Q) \iff Q \ne \Lambda$
5. $\text{dequeue}(\text{enqueue}(\Lambda, x)) = \Lambda$
6. $\text{dequeue}(\text{enqueue}(Q, x)) = \text{enqueue}(\text{dequeue}(Q), x) \iff Q \ne \Lambda$
7. $\text{dequeue}(\Lambda) = \text{Undefined (Underflow)}$

Any structural implementation must preserve these mapping axioms natively in $O(1)$ time bounds.

---

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

---

## Complexity Analysis
- **Time Complexity:** $O(1)$ amortized for all primitive sequence operations.
- **Space Complexity:** $O(N)$ bound strictly to sequence cardinality.

> [!important]
> **Array Constraint:** Unlike a Stack, a linear array inherently fails the Queue constraints. If elements are enqueued at index $N$ and dequeued at index $0$, the structural array undergoes "Linear Drift", exhausting capacity despite internal fragmentation. This necessitates Circular buffer mathematics or Linked Node topology.
