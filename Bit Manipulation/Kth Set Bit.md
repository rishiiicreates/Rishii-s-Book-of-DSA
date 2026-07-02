---
type: concept
tags: [bit-manipulation, cpp, bit-masking]
date: 2026-07-01
---
# Check Kth Set Bit

## Problem Statement
Given an integer $N$ and a bit index $K$ (0-indexed from the Least Significant Bit), mathematically evaluate whether the $K$-th bit in the binary representation of $N$ is $1$.

---

## Approach: Boolean Mask Projection

To isolate the $K$-th bit, we construct a **Bit Mask**.
A bit mask is an integer where exactly one specific bit is $1$ and all other bits are $0$.
Mathematically, the integer $2^K$ has exactly the $K$-th bit set. We generate this by left-shifting the integer $1$ by $K$ positions: `mask = 1 << k`.

We then project this mask onto $N$ using the bitwise AND operator ($\land$).
Let $N = \sum_{i=0}^{31} b_i 2^i$.
The expression $N \land (2^K)$ zeroes out all bits $b_i$ where $i \ne K$.
- If $b_K = 1$, the result is $2^K$ (strictly greater than $0$).
- If $b_K = 0$, the result is exactly $0$.

---

## Code Implementation

```cpp
#include <iostream>

using namespace std;

bool isKthBitSet(int n, int k) {
    // Construct mask 2^k and project onto n via AND operator
    // Using 1LL to prevent 32-bit overflow if k >= 31
    return (n & (1LL << k)) != 0;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(1)$ constant ALU execution.
- **Space Complexity:** $O(1)$ auxiliary space.

> [!warning]
> **Left Shift Overflow:** Using `1 << k` evaluates the `1` as a 32-bit signed integer. If $K = 31$, shifting a signed 32-bit integer into the sign bit triggers Undefined Behavior in C++. Always use `1LL << k` or `1U << k` when generating bit masks to guarantee safety and 64-bit space availability.
