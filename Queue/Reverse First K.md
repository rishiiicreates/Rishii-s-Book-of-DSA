---
type: concept
tags: [queue, stack, cpp]
date: 2026-06-30
---
# Reverse First K Elements of Queue

## Problem Statement
Given an integer $K$ and a Queue $Q$ of $N$ integers, mathematically reverse the order of the first $K$ elements of the queue, leaving the remaining $N - K$ elements in their original relative topological order.

*Example:* $Q = [10, 20, 30, 40, 50]$, $K = 3$
*Result:* $Q = [30, 20, 10, 40, 50]$

---

## Approach: LIFO Subsystem Injection

A Queue operates strictly on a First-In-First-Out (FIFO) axiom. Reversing elements inherently requires a Last-In-First-Out (LIFO) invariant. We can satisfy this by utilizing an auxiliary [[Stack]].

1. **Extract and Invert:** Dequeue the first $K$ elements from $Q$ and push them consecutively onto a stack $S$. The stack naturally inverts their topological order.
2. **Reintegrate:** Pop all $K$ elements from $S$ and enqueue them back into $Q$. 
   - *State of Queue:* The first $K$ elements are now reversed but are located at the *back* (tail) of the queue. The original remaining $N - K$ elements are at the front.
3. **Shift Remainder:** To restore absolute positioning, mathematically perform a circular shift on the remaining $N - K$ elements. For $i$ from $0$ to $N - K - 1$, dequeue an element from the front of $Q$ and immediately enqueue it back to the tail of $Q$.

---

## Code Implementation

```cpp
#include <queue>
#include <stack>
using namespace std;

queue<int> modifyQueue(queue<int> q, int k) {
    stack<int> s;
    int n = q.size();
    
    // Safety check
    if (k <= 0 || k > n) return q;
    
    // Step 1: Dequeue first K elements into a stack
    for (int i = 0; i < k; ++i) {
        s.push(q.front());
        q.pop();
    }
    
    // Step 2: Enqueue reversed elements back to the queue
    while (!s.empty()) {
        q.push(s.top());
        s.pop();
    }
    
    // Step 3: Shift the remaining N - K elements
    for (int i = 0; i < n - k; ++i) {
        q.push(q.front());
        q.pop();
    }
    
    return q;
}
```

> [!important]
> Attempting to solve this problem recursively to achieve $O(1)$ explicit auxiliary space still incurs an implicit $O(K)$ space penalty via the call stack. The iterative stack-based solution is generally preferred to prevent stack overflow on large values of $K$.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. We perform exactly $K$ operations to push to the stack, $K$ operations to pop from the stack, and $N - K$ operations to circularly shift the remainder. This is precisely $O(K + K + N - K) = O(N)$ constant-time queue/stack operations.
- **Space Complexity:** $O(K)$. The auxiliary stack maintains exactly $K$ elements at its maximum capacity.
