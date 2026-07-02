---
type: concept
tags: [cpp, number-theory]
date: 2026-06-30
---
# Sieve of Eratosthenes

## Problem Statement
Find all prime numbers up to a given limit $N$.

## Approach / Intuition
Create a boolean array indicating prime status. Start from 2, and for each prime found, mark its multiples as composite. Optimization: start marking from $p^2$ as smaller multiples are already marked by smaller [[Prime Numbers]].

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N \log \log N)$
- **[[Space Complexity]]:** $O(N)$

## Sample Code
```cpp
vector<bool> sieve(int n) {
    vector<bool> isPrime(n + 1, true);
    isPrime[0] = isPrime[1] = false;
    for (int p = 2; p * p <= n; p++) {
        if (isPrime[p]) {
            for (int i = p * p; i <= n; i += p)
                isPrime[i] = false;
        }
    }
    return isPrime;
}
```

## New Keywords / STL Used
`vector<bool>`

## Edge Cases
$N < 2$, small ranges.
