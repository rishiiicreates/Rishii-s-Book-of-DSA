---
type: concept
tags: [searching, cpp, math]
date: 2026-07-01
---
# Check Power

## Problem Statement
Given two integers $X$ and $Y$, determine if $X$ is a perfect power of $Y$. Mathematically, does there exist an integer $k \ge 0$ such that $Y^k = X$?

---

## Approach: Exponential Scaling

While this might seem like a binary search problem at a glance, since we are iterating over the exponent $k$, the values grow exponentially. The maximum possible value for $k$ is extremely small (e.g., if $Y=2$, $k \le 60$ for 64-bit integers; if $Y \ge 3$, $k$ is even smaller).

Therefore, binary search is overkill. We can simply scale a variable $P = 1$ by repeatedly multiplying it by $Y$ until it equals or exceeds $X$.

1. Start with $P = 1$.
2. While $P < X$, multiply $P$ by $Y$.
3. Check if $P == X$.

To prevent integer overflow on the final multiplication, we must guard the multiplication: we only compute $P \times Y$ if $P \le \frac{X}{Y}$.

---

## Code Implementation

```cpp
#include <iostream>

using namespace std;

bool isPower(long long x, long long y) {
    // Base cases based on the mathematical properties of 1
    if (x == 1) return true;       // y^0 = 1 for all y
    if (y == 1) return x == 1;     // 1^k = 1, so x must be 1
    
    long long p = 1;
    
    while (p < x) {
        // Prevent overflow before multiplying
        if (p > x / y) break;
        p *= y;
    }
    
    return p == x;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(\log_Y X)$. In the worst case ($Y=2$), the loop runs at most 60 times.
- **Space Complexity:** $O(1)$ auxiliary space.

> [!note]
> **Base Case Importance:** The base case $Y = 1$ is an infinite loop hazard if not handled properly. If $Y=1$ and $X > 1$, $P$ starts at $1$ and repeatedly multiplies by $1$, meaning $P < X$ always evaluates to true, freezing the program.
