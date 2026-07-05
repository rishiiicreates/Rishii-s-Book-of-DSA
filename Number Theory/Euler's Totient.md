# Euler's Totient

## Problem Statement
- calculate the number of positive integers up to $N$ that are relatively prime to $N$.

## Approach / Intuition
- use the formula $\phi(n) = n \times (1 - \frac{1}{p_1}) \times \dots \times (1 - \frac{1}{p_k})$ where $p_i$ are the distinct prime factors of $N$. This can be computed by iterating up to $\sqrt{N}$ to find [[Prime Factorization]].

## Time & Space Complexity
- **[[time Complexity]]:** $O(\sqrt{N})$
- **[[space Complexity]]:** $O(1)$

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
- none

## Edge Cases
- $n = 1$, prime numbers.

NEXT: [[Index]]
