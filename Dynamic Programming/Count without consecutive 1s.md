# Count without consecutive 1s

## Problem Statement
- count the number of binary strings of length $n$ that do not contain consecutive 1s.

## Approach / Intuition
- let $dp[0][i]$ be the count of valid strings of length $i$ ending in 0, and $dp[1][i]$ be the count ending in 1. If we append 0, the previous bit could be 0 or 1. If we append 1, the previous bit must be 0. Using [[Dynamic Programming]], the transitions are $dp[0][i] = dp[0][i-1] + dp[1][i-1]$ and $dp[1][i] = dp[0][i-1]$. This is equivalent to finding the $(n+2)$-th [[Fibonacci]] number.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$ using iteration, can be optimized to $O(\log N)$ with [[Matrix Exponentiation]].
- **[[space Complexity]]:** $O(1)$ using two variables.

## Sample Code
```cpp
long long countStrings(int n) {
    long long endsWith0 = 1;
    long long endsWith1 = 1;
    for (int i = 2; i <= n; i++) {
        long long next0 = endsWith0 + endsWith1;
        long long next1 = endsWith0;
        endsWith0 = next0;
        endsWith1 = next1;
    }
    return endsWith0 + endsWith1;
}
```

## New Keywords / STL Used
- none.

## Edge Cases
- $n = 0$, $n = 1$, large $n$ requiring modulo arithmetic.

NEXT: [[Index]]
