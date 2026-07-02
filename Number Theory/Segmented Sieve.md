---
type: concept
tags: [cpp, number-theory]
date: 2026-06-30
---
# Segmented Sieve

## Problem Statement
Find all primes in a range $[L, R]$ where $R$ can be up to $10^{12}$, but $R - L \le 10^6$.

## Approach / Intuition
Use a standard [[Sieve of Eratosthenes]] to find primes up to $\sqrt{R}$. Then use these primes to mark multiples in the offset range $[L, R]$ using an array of size $R - L + 1$.

## Time & Space Complexity
- **[[Time Complexity]]:** $O((R-L + \sqrt{R}) \log \log \sqrt{R})$
- **[[Space Complexity]]:** $O(\sqrt{R} + R - L)$

## Sample Code
```cpp
vector<int> segmentedSieve(long long L, long long R) {
    long long limit = sqrt(R);
    vector<bool> isPrime(limit + 1, true);
    vector<int> primes;
    for (long long i = 2; i <= limit; ++i) {
        if (isPrime[i]) {
            primes.push_back(i);
            for (long long j = i * i; j <= limit; j += i)
                isPrime[j] = false;
        }
    }
    
    vector<bool> isPrimeRange(R - L + 1, true);
    if (L == 1) isPrimeRange[0] = false;
    
    for (long long p : primes) {
        long long start = max(p * p, (L + p - 1) / p * p);
        for (long long j = start; j <= R; j += p)
            isPrimeRange[j - L] = false;
    }
    
    vector<int> primesInRange;
    for (long long i = 0; i <= R - L; ++i) {
        if (isPrimeRange[i]) primesInRange.push_back(i + L);
    }
    return primesInRange;
}
```

## New Keywords / STL Used
`sqrt`, `max`

## Edge Cases
$L = 1$, large $R$ nearing limits.
