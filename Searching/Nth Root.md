---
type: concept
tags: [searching, cpp, binary-search, math]
date: 2026-07-01
---
# Nth Root

## Problem Statement
Given two positive integers $N$ and $M$, find the exact integer $N$-th root of $M$. If $M$ does not have a perfect integer $N$-th root, return $-1$. Mathematically, find $k \ge 1$ such that $k^N = M$.

---

## Approach: Bounded Exponentiation Search

The function $f(k) = k^N$ is strictly monotonically increasing for $k \ge 1, N \ge 1$.
Because of this monotonicity, we can binary search the potential roots in the range $[1, M]$.

1. Maintain pointers $L = 1$ and $R = M$.
2. For midpoint $mid$:
   - If $mid^N == M$, return $mid$.
   - If $mid^N < M$, search right ($L = mid + 1$).
   - If $mid^N > M$, search left ($R = mid - 1$).

The core difficulty is efficiently and safely computing $mid^N$. If we simply use `pow()`, precision issues might arise. If we use a naive loop, we risk massive integer overflow (e.g., $1000^{10} = 10^{30}$, far exceeding a 64-bit integer limit of $\approx 9 \times 10^{18}$).
We must implement a **capped exponentiation** function that terminates immediately if the product strictly exceeds $M$.

---

## Code Implementation

```cpp
#include <iostream>

using namespace std;

// 1 = exact match, 0 = too small, 2 = strictly exceeded M
int checkPowerState(int mid, int n, int m) {
    long long current = 1;
    for (int i = 1; i <= n; i++) {
        current *= mid;
        // Early termination to absolutely prevent 64-bit overflow
        if (current > m) return 2; 
    }
    if (current == m) return 1;
    return 0;
}

int NthRoot(int n, int m) {
    int low = 1, high = m;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        int state = checkPowerState(mid, n, m);
        
        if (state == 1) {
            return mid;
        } else if (state == 0) {
            low = mid + 1; // mid^n < m
        } else {
            high = mid - 1; // mid^n > m
        }
    }
    
    return -1;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(\log M \cdot N)$. The binary search executes $O(\log M)$ times. In the absolute worst case, the exponentiation evaluates $N$ multiplications.
- **Space Complexity:** $O(1)$ auxiliary space.

> [!important]
> **Why `current > m` termination is flawless:** We only care if $mid^N$ equals $M$. The moment the accumulating product exceeds $M$, it's mathematically impossible for further multiplications (by an integer $\ge 1$) to bring it back down to $M$. Returning immediately saves time and prevents UB from standard integer overflow.
