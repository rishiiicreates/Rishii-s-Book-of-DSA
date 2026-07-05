# Deque Theory & Axiomatization

## Problem Statement
- formally define and axiomatize the Double-Ended Queue (Deque) abstract data type (ADT), extending both LIFO and FIFO constraints into a unified bidirectional sequence structure.


## Approach: Bidirectional Sequence Mathematics

- a Deque $D$ over a domain of elements $E$ relaxes the rigid boundary constraints of Stacks and Queues. It mathematically models a strictly topologically ordered sequence where structural mutations (insertions and deletions) are permitted at *both* boundary extrema (Front and Rear), but strictly forbidden in the sequence interior.

- **axiomatic Definition:**
- let $D$ be a deque state, $x \in E$, and $\Lambda$ denote the empty deque.
- we define the primitive dual-boundary operations:
- $\text{push\_front}(D, x) \to D'$
- $\text{push\_back}(D, x) \to D''$
- $\text{pop\_front}(D) \to D'''$
- $\text{pop\_back}(D) \to D''''$
- $\text{front}(D) \to x$
- $\text{back}(D) \to y$

- **algebraic Axioms:**
- the Deque subsumes both LIFO and FIFO axioms perfectly:
- $\text{front}(\text{push\_front}(D, x)) = x$
- $\text{back}(\text{push\_back}(D, x)) = x$
- $\text{pop\_front}(\text{push\_front}(D, x)) = D$
- $\text{pop\_back}(\text{push\_back}(D, x)) = D$
- $\text{front}(\text{push\_back}(\Lambda, x)) = \text{back}(\text{push\_back}(\Lambda, x)) = x$

- a structurally valid implementation must map all four boundary operations natively in $O(1)$ time.


## Standard Template Library (STL) Implementation

```cpp
#include <iostream>
#include <deque>

using namespace std;

void stlDequeExample() {
    // std::deque is a primary sequential container in C++
    deque<int> dq;
    
    dq.push_back(10);  // D = [10]
    dq.push_front(20); // D = [20, 10]
    dq.push_back(30);  // D = [20, 10, 30]
    
    int f = dq.front(); // Returns 20
    int b = dq.back();  // Returns 30
    
    dq.pop_front(); // D = [10, 30]
    dq.pop_back();  // D = [10]
    
    bool is_empty = dq.empty(); // Returns false
}
```


## Complexity Analysis
- **time Complexity:** $O(1)$ strictly exact (or amortized for dynamic scaling) for all bidirectional boundary mutations.
- **space Complexity:** $O(N)$ bound strictly to sequence cardinality.

> [!important]
> **Memory Architecture in C++:** Unlike `std::vector` (a single contiguous block) or `std::list` (scattered disjoint nodes), `std::deque` is mathematically implemented as an array of pointers to fixed-size contiguous memory chunks. This permits $O(1)$ boundary scaling in *both* directions without the catastrophic $O(N)$ reallocation overhead of vectors, while retaining superior cache locality compared to linked lists.

NEXT: [[Index]]
