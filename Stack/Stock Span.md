---
type: concept
tags: [stack, monotonic-stack, cpp, array, math]
date: 2026-06-30
---
# Stock Span Problem

## Problem Statement
Given a discrete temporal sequence of stock prices, mathematically evaluate the theoretical "span" of each price. The span $S[i]$ is structurally defined as the maximum contiguous number of days immediately preceding (and including) day $i$ for which the price is mathematically $\le$ the price on day $i$.

---

## Approach: Temporal Distance via Monotonic Stack

This problem theoretically reduces to discovering the index of the **Previous Greater Element**. If the closest preceding strictly greater element resides at index $j$, the continuous contiguous span bounded by it evaluates algebraically to:
$$ S[i] = i - j $$
If no such structurally superior bound exists, the price is an absolute historical maximum, evaluating to a span of $i + 1$.

Algorithm:
1. Maintain a [[Monotonic Stack]] storing the spatial **indices** (not the raw scalar values) of elements to accurately evaluate temporal distance.
2. Enforce a strictly decreasing mathematical invariant on the stack based on raw price values.
3. For each day $i$, forcefully evict any index $k$ from the stack where $\text{price}[k] \le \text{price}[i]$.
4. The resolved index remaining on the stack represents the strict boundary $j$.
5. Evaluate $S[i] = i - j$. If the stack is exhausted, evaluate $S[i] = i + 1$.
6. Push index $i$ to constraint future evaluations.

---

## Code Implementation

```cpp
#include <vector>
#include <stack>

using namespace std;

vector<int> calculateSpan(const vector<int>& price) {
    int n = price.size();
    vector<int> span(n, 0);
    stack<int> s; // LIFO structure maintaining temporal indices
    
    for (int i = 0; i < n; i++) {
        // Evict mathematically inferior structural bounds
        while (!s.empty() && price[s.top()] <= price[i]) {
            s.pop();
        }
        
        // Algebraically evaluate spatial span
        if (s.empty()) {
            span[i] = i + 1; // Absolute historical maximum
        } else {
            span[i] = i - s.top(); // Relative structural boundary
        }
        
        // Constrain future geometric evaluations
        s.push(i);
    }
    
    return span;
}
```

> [!important]
> Storing the indices rather than the direct scalar values is structurally mandatory for all mathematical problems demanding distance, span, width, or absolute spatial boundaries. The raw value can always be accessed via `price[index]`, but the inverse is structurally impossible in $\mathcal{O}(1)$.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$ amortized. The total geometric push and pop operations strictly sum to $2N$.
- **Space Complexity:** $\mathcal{O}(N)$. The LIFO structure is theoretically bounded by the sequence size.
