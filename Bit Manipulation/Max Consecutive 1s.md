---
type: concept
tags: [bit-manipulation, cpp, math, bit-shifting]
date: 2026-07-01
---
# Max Consecutive 1s

## Problem Statement
Given an integer $N$, compute the maximum topological length of consecutive set bits (`1`s) in its binary representation.

---

## Approach: Bitwise Intersection

While a traditional loop checking each bit works in $O(\log N)$, we can apply an elegant intersection algorithm.
If we left-shift $N$ by 1, $N \ll 1$, we displace all bits by one position.
If we take the bitwise AND:
$$ N' = N \land (N \ll 1) $$
Every block of consecutive $1$s of length $L$ in $N$ will overlap with itself shifted by 1, yielding a new block of consecutive $1$s of length exactly $L - 1$.
Isolated $1$s (length 1) will fail to overlap and will collapse to $0$.

Thus, each application of $N \leftarrow N \land (N \ll 1)$ strictly decrements the length of all consecutive $1$ sequences by exactly 1.
The total number of iterations required to collapse $N$ to exactly $0$ is precisely equal to the length of the longest consecutive sequence.

---

## Code Implementation

```cpp
#include <iostream>

using namespace std;

int maxConsecutiveOnes(int n) {
    int count = 0;
    
    // Iteratively collapse consecutive blocks
    while (n != 0) {
        n = (n & (n << 1));
        count++;
    }
    
    return count;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(K)$, where $K$ is the maximum sequence length of consecutive `1`s. Bounded strictly by $O(\log N)$ in the worst case (all 1s).
- **Space Complexity:** $O(1)$ auxiliary space.

> [!warning]
> **Negative Integers:** If $N$ is negative, left shifting is technically undefined behavior in C++, though it typically operates as an arithmetic shift. The algorithm assumes Two's Complement representation, where $-1$ is an infinite sequence of $1$s. Ensure $N$ is treated as an `unsigned int` to prevent infinite loops from sign bit regeneration.
