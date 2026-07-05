# Stack Theory & Axiomatization

## Problem Statement
- formally define and axiomatize the Stack abstract data type (ADT) enforcing the strict Last-In-First-Out (LIFO) topological ordering.


## Approach: Axiomatic Semantics (LIFO)

- a Stack $S$ over a domain of elements $E$ is mathematically defined as a restricted sequence where insertions and deletions operate exclusively at a single boundary, denoted as the `Top`.
- this enforces the temporal constraint: the element resident in the sequence for the shortest duration is the first to be extracted.

- **axiomatic Definition:**
- let $S$ be a stack state, $x \in E$, and $\Lambda$ denote the empty stack.
- we define the primitive operations:
- $\text{push}(S, x) \to S'$
- $\text{pop}(S) \to S''$
- $\text{top}(S) \to x$
- $\text{empty}(S) \to \text{Boolean}$

- **algebraic Axioms:**
- $\text{empty}(\Lambda) = \text{True}$
- $\text{empty}(\text{push}(S, x)) = \text{False}$
- $\text{top}(\text{push}(S, x)) = x$
- $\text{pop}(\text{push}(S, x)) = S$
- $\text{pop}(\Lambda) = \text{Undefined (Underflow)}$
- $\text{top}(\Lambda) = \text{Undefined (Underflow)}$

- any valid structural implementation (Array or Linked List) must strictly preserve these axioms mapping state transitions in $O(1)$ time.


## Standard Template Library (STL) Implementation

```cpp
#include <iostream>
#include <stack>

using namespace std;

void stlStackExample() {
    // Under the hood, std::stack is a container adaptor.
    // By default, it wraps std::deque to satisfy LIFO axioms.
    stack<int> s;
    
    s.push(10); // S = [10]
    s.push(20); // S = [10, 20]
    
    int top_val = s.top(); // Returns 20
    s.pop();               // S = [10]
    
    bool is_empty = s.empty(); // Returns false
}
```


## Complexity Analysis
- **time Complexity:** $O(1)$ amortized for all primitive operations.
- **space Complexity:** $O(N)$ auxiliary storage bound strictly to the element cardinality.

> [!tip]
> **Container Adaptor Architecture:** `std::stack` is not an atomic data structure. It is an adaptor that can wrap any underlying sequence container providing `push_back`, `pop_back`, and `back` (e.g., `std::vector`, `std::deque`, `std::list`). `std::deque` is preferred as it dynamically allocates fixed-size chunks, preventing the mass memory reallocation stalling associated with `std::vector`.

NEXT: [[Index]]
