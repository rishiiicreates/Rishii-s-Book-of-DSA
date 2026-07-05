# Previous Smaller Element

## Problem Statement
- given a discrete sequence $A$ of length $N$, mathematically determine for each temporal state $A[i]$ the closest preceding structural element $A[j]$ (where $j < i$) that satisfies the strict inequality $A[j] < A[i]$. Output $-1$ if no such lower bound exists.


## Approach: Strictly Increasing Monotonic Stack

- we can theoretically resolve this spatial query in strictly linear time by maintaining an invariant [[Monotonic Stack]].
- by enforcing a strictly increasing geometric constraint on the stack, we efficiently prune structurally irrelevant historical data. If an element $A[k]$ arrives, it completely eclipses any historically larger element $A[j]$ (where $j < k$ and $A[j] \ge A[k]$), because for any future query $A[i]$ (where $i > k$), $A[k]$ acts as a superior, closer, and smaller constraint.

- algorithm:
- initialize an empty LIFO stack to theoretically track lower geometric boundaries.
- traverse the temporal sequence from $i = 0$ to $N-1$.
- while the top of the stack is theoretically $\ge A[i]$, pop it. It violates the increasing mathematical invariant and is structurally irrelevant for current and future states.
- the resolved stack top is inherently the closest strict geometric lower bound.
- push $A[i]$ into the LIFO structure.


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


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$ amortized. Each temporal state is geometrically pushed and popped strictly at most once.
- **space Complexity:** $\mathcal{O}(N)$ auxiliary worst-case bound, occurring strictly on a monotonically ascending input sequence.

NEXT: [[Index]]
