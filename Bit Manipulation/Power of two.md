---
type: concept
tags: [bit-manipulation, cpp, math]
date: 2026-07-01
---
# Power of Two

## Problem Statement
Given a mathematical integer $N$, evaluate whether $N$ is a perfect power of two. Return `true` if $\exists k \ge 0$ such that $2^k = N$, else return `false`.

---

## Approach: Brian Kernighan's Set Bit Isolation

A brute force division algorithm takes $O(\log N)$ time.
We can solve this in strict $O(1)$ by analyzing the binary representation of powers of two.
Any mathematical power of two $N = 2^k$ contains exactly **one** bit set to `1`, followed by exactly $k$ zeroes.
Example for $N = 8$: `1000`

If we subtract $1$ from a power of two, $N-1$, it borrows from the single set bit, inverting all trailing bits up to and including the set bit.
Example for $N-1 = 7$: `0111`

If we apply the bitwise AND operator between $N$ and $N-1$, the result mathematically annihilates to $0$:
$$ N \land (N - 1) = \texttt{1000} \land \texttt{0111} = 0 $$

If $N$ has multiple set bits, $N-1$ only flips bits up to the *lowest* set bit. The higher set bits remain unaffected, meaning $N \land (N-1)$ will definitely be non-zero.

---

## Code Implementation

```cpp
#include <iostream>

using namespace std;

bool isPowerOfTwo(int n) {
    // 1. N must be strictly positive (2^k > 0 for all real k)
    // 2. Annihilation of the single set bit must yield 0
    return n > 0 && (n & (n - 1)) == 0;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(1)$ constant evaluation.
- **Space Complexity:** $O(1)$ auxiliary space.

> [!important]
> **Zero and Negatives:** Mathematical precision is critical for the `n > 0` condition. If $N = 0$, $0 \land -1 = 0$, which incorrectly suggests $0$ is a power of two. Furthermore, negative numbers in Two's Complement cannot be powers of $2$, and evaluating them risks undefined logical behaviors in broader contexts.
