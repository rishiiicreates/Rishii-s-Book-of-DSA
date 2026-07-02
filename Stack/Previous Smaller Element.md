---
type: concept
tags: [stack, monotonic-stack, cpp, array, math]
date: 2026-06-30
---
# Previous Smaller Element

## Problem Statement
Given a discrete sequence $A$ of length $N$, mathematically determine for each temporal state $A[i]$ the closest preceding structural element $A[j]$ (where $j < i$) that satisfies the strict inequality $A[j] < A[i]$. Output $-1$ if no such lower bound exists.

---

## Approach: Strictly Increasing Monotonic Stack

We can theoretically resolve this spatial query in strictly linear time by maintaining an invariant [[Monotonic Stack]].
By enforcing a strictly increasing geometric constraint on the stack, we efficiently prune structurally irrelevant historical data. If an element $A[k]$ arrives, it completely eclipses any historically larger element $A[j]$ (where $j < k$ and $A[j] \ge A[k]$), because for any future query $A[i]$ (where $i > k$), $A[k]$ acts as a superior, closer, and smaller constraint.

Algorithm:
1. Initialize an empty LIFO stack to theoretically track lower geometric boundaries.
2. Traverse the temporal sequence from $i = 0$ to $N-1$.
3. While the top of the stack is theoretically $\ge A[i]$, pop it. It violates the increasing mathematical invariant and is structurally irrelevant for current and future states.
4. The resolved stack top is inherently the closest strict geometric lower bound.
5. Push $A[i]$ into the LIFO structure.

---

## Code Implementation

```cpp
#include <vector>
#include <stack>

using namespace std;

vector<int> previousSmallerElement(const vector<int>& arr) {
    int n = arr.size();
    vector<int> res(n, -1);
    stack<int> s; // Enforces strictly increasing spatial lower bounds
    
    for (int i = 0; i < n; i++) {
        // Forceful eviction of mathematically inferior (larger) constraints
        while (!s.empty() && s.top() >= arr[i]) {
            s.pop();
        }
        
        // Record structural resolution
        if (!s.empty()) {
            res[i] = s.top();
        }
        
        // Push resolved temporal state
        s.push(arr[i]);
    }
    
    return res;
}
```

> [!tip]
> A memory-optimized variant structurally leverages the pre-allocated result array `res` or a direct index-based array as a virtual stack to strictly bounded auxiliary $\mathcal{O}(N)$ memory allocations.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$ amortized. Each temporal state is geometrically pushed and popped strictly at most once.
- **Space Complexity:** $\mathcal{O}(N)$ auxiliary worst-case bound, occurring strictly on a monotonically ascending input sequence.
