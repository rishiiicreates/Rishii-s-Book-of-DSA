# Reverse a Queue

## Problem Statement
- given a structurally populated Queue ADT $Q$, strictly invert the topological sequence of all elements. E.g., $Q = [1, 2, 3, 4] \implies [4, 3, 2, 1]$.


## Approach: LIFO Stack Inversion

- because a Queue enforces FIFO and cannot access interior elements, to invert the temporal sequence, we must leverage a secondary structure that mathematically enforces LIFO topology—specifically, a Stack.

- **iterative Stack Machine:**
- systematically extract (`dequeue`) every scalar from the Front of the Queue $Q$ and topologically `push` it onto an auxiliary Stack $S$. (This drains $Q$ and sequences $S$ in LIFO).
- the absolute oldest element is now bounded at the geometric bottom of the stack, and the most recent is at the topological peak.
- systematically extract (`pop`) every scalar from the peak of $S$ and `enqueue` it back into $Q$.
- the LIFO extraction forces the most recently enqueued elements to enter $Q$ first, mathematically inverting the sequence.

- **recursive Call-Stack Alternative:**
- the auxiliary Stack space can be implicitly mapped directly into the system's execution call-stack via recursive depth bounding.


## Code Implementation (Iterative LIFO Bounding)

```cpp
#include <queue>
#include <stack>

using namespace std;

void reverseQueue(queue<int>& q) {
    stack<int> st;
    
    // Drain sequence topologically mapping FIFO -> LIFO
    while (!q.empty()) {
        st.push(q.front());
        q.pop();
    }
    
    // Drain auxiliary bounds mapping LIFO -> inverted FIFO
    while (!st.empty()) {
        q.push(st.top());
        st.pop();
    }
}
```

## Code Implementation (Recursive System Mapping)

```cpp
#include <queue>

using namespace std;

void reverseQueueRecursive(queue<int>& q) {
    // Base Case Boundary
    if (q.empty()) return;
    
    // Extract topological temporal scalar
    int val = q.front();
    q.pop();
    
    // Depth-bound recursion forces call-stack LIFO suspension
    reverseQueueRecursive(q);
    
    // LIFO unwind injection
    q.push(val);
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ linear extraction and sequential injection for both topologies.
- **space Complexity:** $O(N)$ auxiliary space either strictly mapped to the internal `std::stack` or implicitly bounded by OS process Call-Stack depth.

> [!warning]
> **Recursive Depth Danger:** While mathematically elegant, the recursive mapping algorithm forces $O(N)$ discrete system stack frames. For sequences exceeding local OS thread stack bounds ($\approx 8$MB), this structure mathematically guarantees a catastrophic memory Fault (Stack Overflow) abort, whereas the iterative heap `std::stack` bounds safely scale to generic RAM limits.

NEXT: [[Index]]
