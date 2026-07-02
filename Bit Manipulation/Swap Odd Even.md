---
type: concept
tags: [bit-manipulation, cpp, bit-masking, math]
date: 2026-06-30
---
# Swap Odd and Even Bits

## Problem Statement
Given an unsigned 32-bit integer $N$, swap its theoretically adjacent even and odd bits. (Bit $0$ swaps with Bit $1$, Bit $2$ swaps with Bit $3$, etc.).

---

## Approach: Orthogonal Structural Isolation

We can structurally isolate the two disjoint sets of bits (even indexed and odd indexed) using predetermined 32-bit hexadecimal masks.
1. **Even Mask Isolation:** The mask `0xAAAAAAAA` (which is `10101010...` in binary) structurally isolates all bits residing on odd indices (note: the indices are 0-based, so $1, 3, 5, \dots$ are isolated by `A`=$10$).
2. **Odd Mask Isolation:** The mask `0x55555555` (which is `01010101...` in binary) structurally isolates all bits residing on even indices ($0, 2, 4, \dots$ isolated by `5`=$01$).

Algorithm:
- Extract the mathematically isolated components.
- Shift the even-indexed bits strictly to the left by $1$ (pushing them into odd indices).
- Shift the odd-indexed bits strictly to the right by $1$ (pushing them into even indices).
- Structurally recombine the shifted sets using a bitwise `OR` (`|`).

---

## Code Implementation

```cpp
unsigned int swapOddEvenBits(unsigned int n) {
    // Isolate bits strictly situated at odd indices (1, 3, 5...)
    // 0xA = 1010
    unsigned int oddIndexBits = n & 0xAAAAAAAA;
    
    // Isolate bits strictly situated at even indices (0, 2, 4...)
    // 0x5 = 0101
    unsigned int evenIndexBits = n & 0x55555555;
    
    // Shift odd-indexed bits right to even positions
    oddIndexBits >>= 1;
    
    // Shift even-indexed bits left to odd positions
    evenIndexBits <<= 1;
    
    // Recombine orthogonally
    return oddIndexBits | evenIndexBits;
}
```

> [!tip]
> A common conceptual pitfall is confusion between "Even Bits" vs "Even Indices". In a 0-indexed system, the bit representing $2^0$ is at index $0$ (even index), the bit representing $2^1$ is at index $1$ (odd index). `0xAAAAAAAA` mathematically targets the odd *indices*.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(1)$. Evaluated strictly in deterministic bounded hardware cycles.
- **Space Complexity:** $\mathcal{O}(1)$. Entirely constant auxiliary space bounded strictly by register mapping.
