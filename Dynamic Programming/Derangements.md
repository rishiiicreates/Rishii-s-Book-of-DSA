# Derangements

## Problem Statement
- count the number of derangements of $n$ items, which is a permutation of the elements of a set, such that no element appears in its original position.

## Approach / Intuition
- let $D(n)$ be the number of derangements of $n$ items. When we place the $n$-th item at position $i$ (where $i \neq n$), we have $n-1$ choices for $i$. The item originally at position $i$ can either be placed at position $n$, leaving a derangement of $n-2$ items, or it can be placed anywhere except position $n$, which is equivalent to a derangement of $n-1$ items. Thus, $D(n) = (n - 1) \times (D(n - 1) + D(n - 2))$. This is solvable via [[Dynamic Programming]].

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$
- **[[space Complexity]]:** $O(1)$ by keeping track of the last two values.

## Sample Code
```cpp
long long countDerangements(int n) {
    if (n == 1) return 0;
    if (n == 2) return 1;
    long long prev2 = 0;
    long long prev1 = 1;
    long long mod = 1e9 + 7;
    for (int i = 3; i <= n; ++i) {
        long long curr = (i - 1) * (prev1 + prev2) % mod;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

## New Keywords / STL Used
- modulo arithmetic for large numbers.

## Edge Cases
- $n = 1$ (0 derangements), $n = 2$ (1 derangement), large $n$ requiring modulo.

NEXT: [[Index]]
