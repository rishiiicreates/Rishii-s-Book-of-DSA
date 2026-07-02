---
type: concept
tags: [stack, monotonic-stack, cpp, array, math]
date: 2026-06-30
---
# Previous Greater Element

## Problem Statement
Given an array sequence $A$ of length $N$, mathematically evaluate for each element $A[i]$ the first preceding element $A[j]$ (where $j < i$) such that $A[j] > A[i]$. If no such superior element exists in the preceding temporal history, output $-1$.

---

## Approach: Strictly Decreasing Monotonic Stack

A brute-force mathematical evaluation requires $\mathcal{O}(N^2)$ backward scanning. However, we can construct an optimal $\mathcal{O}(N)$ solution by exploiting spatial monotonicity.
If we encounter an element $A[j]$, any subsequent element $A[k]$ (where $k > j$) that is strictly smaller than $A[j]$ cannot possibly act as the "Previous Greater Element" for any future element $A[i]$ if $A[i]$ is itself larger than $A[k]$. This implies smaller structural elements are mathematically eclipsed by larger, more recent elements.

Algorithm:
1. Maintain a [[Stack]] enforcing a strictly decreasing mathematical invariant.
2. Iterate through $A$ temporally from left to right (index $i = 0$ to $N-1$).
3. For each $A[i]$, evaluate the top of the stack. If the stack top is $\le A[i]$, it is structurally eclipsed and mathematically useless for all future queries. Pop it.
4. Once the inferior elements are forcefully evicted, the remaining stack top represents the absolute nearest strictly greater geometric bound.
5. Record this boundary. Push the current $A[i]$ onto the stack to establish a potential boundary for future evaluations.

---

## Code Implementation

```cpp
#include <vector>
#include <stack>

using namespace std;

vector<int> previousGreaterElement(const vector<int>& arr) {
    int n = arr.size();
    vector<int> res(n, -1);
    stack<int> s; // Maintains strictly decreasing temporal states
    
    for (int i = 0; i < n; i++) {
        // Mathematical eviction of structurally eclipsed elements
        while (!s.empty() && s.top() <= arr[i]) {
            s.pop();
        }
        
        // Evaluate geometric boundary
        if (!s.empty()) {
            res[i] = s.top();
        }
        
        // Push current temporal state
        s.push(arr[i]);
    }
    
    return res;
}
```

> [!important]
> The equality constraint within the eviction loop (`s.top() <= arr[i]`) is mathematically required. If a strictly greater element is mandated, an identical equivalent element cannot serve as a valid bound, and must be eliminated.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. Although nested structurally, each element $A[i]$ is theoretically pushed exactly once and popped at most once, bounding the absolute operation count to $2N$.
- **Space Complexity:** $\mathcal{O}(N)$. The LIFO structure geometrically bounds a monotonically decreasing subarray, which evaluates to size $N$ in a worst-case strictly descending input.
