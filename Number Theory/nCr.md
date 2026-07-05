# nCr

## Problem Statement
- calculate combinations nCr modulo a prime (typically 10^9+7).

## Approach / Intuition
- precompute factorials and their modular inverses using [[Fermat's Little Theorem]]. Then calculate nCr efficiently as $(n! \times \text{inv}(r!) \times \text{inv}((n-r)!)) \bmod p$.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N \log M)$ for precomputation, $O(1)$ per query
- **[[space Complexity]]:** $O(N)$ for storing factorials and inverses

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

long long modInverse(long long n, long long p) {
    return power(n, p - 2, p);
}

long long nCr(long long n, long long r, long long p, vector<long long>& fact) {
    if (n < r) return 0;
    if (r == 0) return 1;
    return (fact[n] * modInverse(fact[r], p) % p * modInverse(fact[n - r], p) % p) % p;
}
```

## New Keywords / STL Used
- `vector`

## Edge Cases
- $n < r$, $r = 0$, $n = 0$, modulo overflow.

NEXT: [[Index]]
