---
type: concept
tags: [cpp, number-theory]
date: 2026-06-30
---
# Euler's Totient

## Problem Statement
Calculate the number of positive integers up to $N$ that are relatively prime to $N$.

## Approach / Intuition
Use the formula $\phi(n) = n \times (1 - \frac{1}{p_1}) \times \dots \times (1 - \frac{1}{p_k})$ where $p_i$ are the distinct prime factors of $N$. This can be computed by iterating up to $\sqrt{N}$ to find [[Prime Factorization]].

## Time & Space Complexity
- **[[Time Complexity]]:** $O(\sqrt{N})$
- **[[Space Complexity]]:** $O(1)$

## Sample Code
```cpp
int eulerTotient(int n) {
    int result = n;
    for (int p = 2; p * p <= n; ++p) {
        if (n % p == 0) {
            while (n % p == 0)
                n /= p;
            result -= result / p;
        }
    }
    if (n > 1)
        result -= result / n;
    return result;
}
```

## New Keywords / STL Used
None

## Edge Cases
$N = 1$, prime numbers.
