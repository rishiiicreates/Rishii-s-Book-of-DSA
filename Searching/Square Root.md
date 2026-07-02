---
type: concept
tags: [searching, cpp, binary-search, math]
date: 2026-07-01
---
# Square Root

## Problem Statement
Given a non-negative integer $X$, find its square root rounded down to the nearest integer. Mathematically, find the largest integer $k \ge 0$ such that $k^2 \le X$.

---

## Approach: Monotonic Predicate Binary Search

The problem exhibits a perfect monotonic property. For any integer $y$:
- If $y^2 \le X$, then for all $z < y$, $z^2 \le X$ also holds.
- If $y^2 > X$, then for all $z > y$, $z^2 > X$ also holds.

This allows us to binary search the answer space $[1, X]$.
1. Set the search bounds $L = 1$ and $R = X$.
2. For midpoint $M = L + \lfloor (R - L) / 2 \rfloor$:
   - If $M^2 \le X$, $M$ is a valid candidate. We record it and search the right half ($L = M + 1$) for a potentially larger, better candidate.
   - If $M^2 > X$, $M$ is invalid and too large. We discard it and search the left half ($R = M - 1$).

---

## Code Implementation

```cpp
#include <iostream>

using namespace std;

int mySqrt(int x) {
    if (x == 0 || x == 1) return x;
    
    int low = 1, high = x;
    int ans = 0;
    
    while (low <= high) {
        // Prevent mid calculation overflow
        int mid = low + (high - low) / 2;
        
        // Prevent mid * mid overflow by casting to 64-bit int
        if (1LL * mid * mid <= x) {
            ans = mid;         // Valid candidate, store it
            low = mid + 1;     // Try for a larger value
        } else {
            high = mid - 1;    // Too large, shrink search space
        }
    }
    
    return ans;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(\log X)$. The search space shrinks by half each iteration.
- **Space Complexity:** $O(1)$ auxiliary space.

> [!warning]
> **Integer Overflow:** The expression `mid * mid` is the most dangerous part of this algorithm. If $X = 2^{31}-1$, `mid` can be up to $2^{30}$, meaning `mid * mid` approaches $2^{60}$, which instantly overflows a 32-bit signed integer. Always cast one operand to `long long` (e.g. `1LL * mid * mid`) or evaluate the mathematical equivalent `mid <= x / mid`.
