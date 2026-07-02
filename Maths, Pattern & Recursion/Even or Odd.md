---
type: concept
tags: [maths, pattern, cpp, bitwise, basics]
date: 2026-06-30
---
# Even or Odd

## Problem Statement
Given an integer $N$, determine if it is even or odd.

---

## Approach 1: Modulo Operator (Standard)

The standard mathematical definition of an even number is any integer perfectly divisible by 2.
```cpp
bool isEven(int n) {
    return n % 2 == 0;
}
```
**Complexity:** $O(1)$ Time, $O(1)$ Space.

---

## Approach 2: Bitwise AND (The Pro Way)

In the binary representation of numbers, every bit represents a power of 2 ($2^0, 2^1, 2^2 \dots$). 
Because all powers of 2 (except $2^0 = 1$) are inherently even, the entire "evenness" or "oddness" of a number is dictated entirely by its very last bit (the Least Significant Bit, LSB).

- If LSB is `0`, the number is Even. (e.g., `1010` is 10)
- If LSB is `1`, the number is Odd. (e.g., `1011` is 11)

We can mask the number with `1` using the Bitwise AND operator `&`. This zeroes out all other bits and isolates the LSB.

```cpp
bool isEvenBitwise(int n) {
    // If n & 1 is 0, it is even. If 1, it is odd.
    return (n & 1) == 0;
}
```

> [!tip] 
> Bitwise operations are historically slightly faster than modulo division at the CPU level. In massive loops in competitive programming, `(n & 1)` is the preferred way to check parity.
