---
type: concept
tags: [bit-manipulation, cpp, math, brian-kernighan]
date: 2026-07-01
---
# Count Set (Hamming Weight)

## Problem Statement
Given an integer $N$, compute its Hamming Weight—the exact topological count of set bits (`1`s) in its binary representation.

---

## Approach: Brian Kernighan's Algorithm

While iterating through all 32 bits is $O(1)$ mathematically, we can optimize the execution to depend strictly on $K$, the number of set bits.
Brian Kernighan's algorithm systematically zeroes out the Lowest Set Bit (LSB) in successive operations.

Let $N = \dots b_{k+1} 1 \underbrace{00\dots0}_{k \text{ zeroes}}$.
Subtracting $1$ flips all bits from the LSB downwards:
$N-1 = \dots b_{k+1} 0 \underbrace{11\dots1}_{k \text{ ones}}$.

Taking the bitwise AND operation:
$$ N \land (N - 1) = \dots b_{k+1} 0 \underbrace{00\dots0}_{k \text{ zeroes}} $$
This effectively erases the lowest set bit. We repeat this annihilation until $N = 0$, incrementing a counter at each step.

---

## Code Implementation

```cpp
#include <iostream>

using namespace std;

int countSetBits(int n) {
    int count = 0;
    while (n != 0) {
        n &= (n - 1); // Annihilate the lowest set bit
        count++;
    }
    return count;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(K)$, where $K$ is the Hamming weight of the integer. In the worst case $K = 32$, but on average $K \approx 16$ for 32-bit integers.
- **Space Complexity:** $O(1)$ auxiliary space.

> [!tip]
> **Hardware Acceleration:** Modern x86 processors offer a dedicated hardware instruction `POPCNT` (Population Count). In C++, you can invoke this via the compiler intrinsic `__builtin_popcount(n)` for 32-bit integers, or `__builtin_popcountll(n)` for 64-bit integers. This operates in exactly 1 CPU cycle, bypassing iterative loops entirely.
