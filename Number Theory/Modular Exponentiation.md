---
type: concept
tags: [cpp, number-theory]
date: 2026-06-30
---
# Modular Exponentiation

## Problem Statement
Compute $(x^y) \bmod p$ efficiently.

## Approach / Intuition
Use binary exponentiation ([[Fast Exponentiation]]). Recursively or iteratively square the base and halve the exponent. If the current exponent is odd, multiply the result by the base. Modulo operation is applied at each multiplication to prevent overflow.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(\log y)$
- **[[Space Complexity]]:** $O(1)$

## Sample Code
```cpp
long long power(long long base, long long exp, long long mod) {
    long long res = 1;
    base %= mod;
    while (exp > 0) {
        if (exp % 2 == 1) res = (res * base) % mod;
        base = (base * base) % mod;
        exp /= 2;
    }
    return res;
}
```

## New Keywords / STL Used
None

## Edge Cases
Base 0, exponent 0, modulo 1.
