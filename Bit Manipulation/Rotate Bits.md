---
type: concept
tags: [bit-manipulation, cpp, bit-shift, math]
date: 2026-06-30
---
# Bitwise Rotation (Circular Shift)

## Problem Statement
Mathematically execute a structural bitwise rotation (circular shift) on a 32-bit unsigned integer $N$.
1. Rotate Left by $D$ positions.
2. Rotate Right by $D$ positions.

---

## Approach: Orthogonal Sub-Mask Recombination

Standard bitwise shifts `<<` and `>>` are linear operations—bits that transgress the 32-bit boundary are permanently destroyed.
To simulate a strictly circular shift, we must structurally decompose the operation into two orthogonal linear operations and recombine them via a bitwise `OR` (`|`).

For a Left Rotation by $D$:
1. The bound $D$ must be constrained via modulo $32$, as rotating by $32$ returns the exact structural original.
2. Shift $N$ left by $D$: `(N << D)`. This preserves the lower $(32 - D)$ bits.
3. Shift $N$ right by $(32 - D)$: `(N >> (32 - D))`. This isolates the $D$ bits that mathematically "fell off" the left boundary, shifting them to the extreme right.
4. Structurally fuse the two orthogonal components.

---

## Code Implementation

```cpp
// Strictly requires unsigned int to enforce logical shifting
unsigned int rotateLeft(unsigned int n, int d) {
    // Mathematical boundary truncation
    d %= 32;
    if (d == 0) return n;
    
    // Orthogonal fusion
    return (n << d) | (n >> (32 - d));
}

unsigned int rotateRight(unsigned int n, int d) {
    // Mathematical boundary truncation
    d %= 32;
    if (d == 0) return n;
    
    // Orthogonal fusion
    return (n >> d) | (n << (32 - d));
}
```

> [!important]
> The early return `if (d == 0)` is a strict mathematical necessity in C++. If $D = 0$, evaluating `32 - 0 = 32`, and subsequently shifting a 32-bit integer by $32$ positions `(n >> 32)` fundamentally triggers **Undefined Behavior (UB)** according to the C++ Standard.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(1)$. Resolved via constant-time hardware CPU bitwise operators.
- **Space Complexity:** $\mathcal{O}(1)$. Evaluates exclusively within bounded CPU registers.
