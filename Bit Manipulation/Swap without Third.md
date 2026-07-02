---
type: concept
tags: [bit-manipulation, cpp, math, xor]
date: 2026-07-01
---
# Swap without Third Variable

## Problem Statement
Given two mathematical variables $A$ and $B$, swap their discrete values strictly in-place, without allocating memory for a temporary third variable.

---

## Approach: XOR Symmetric Difference

We can exploit the mathematical properties of the bitwise XOR operator ($\oplus$).
XOR forms an abelian group over the Boolean domain with the following axioms:
1. **Identity:** $X \oplus 0 = X$
2. **Self-Inverse:** $X \oplus X = 0$
3. **Associativity:** $(X \oplus Y) \oplus Z = X \oplus (Y \oplus Z)$

By leveraging these axioms, we can superimpose the information of $A$ and $B$ into a single variable and subsequently extract them.

Let the initial states be $A_0$ and $B_0$.
1. **Step 1:** $A_1 = A_0 \oplus B_0$ (Now $A$ holds the combined state of both)
2. **Step 2:** $B_1 = A_1 \oplus B_0 \implies (A_0 \oplus B_0) \oplus B_0 \implies A_0 \oplus (B_0 \oplus B_0) \implies A_0 \oplus 0 \implies A_0$. (Now $B$ holds the original value of $A$).
3. **Step 3:** $A_2 = A_1 \oplus B_1 \implies (A_0 \oplus B_0) \oplus A_0 \implies (A_0 \oplus A_0) \oplus B_0 \implies 0 \oplus B_0 \implies B_0$. (Now $A$ holds the original value of $B$).

---

## Code Implementation

```cpp
#include <iostream>

using namespace std;

void swapWithoutThird(int& a, int& b) {
    // Crucial check: If a and b are referencing the exact same memory address,
    // a = a ^ a will immediately zero out the variable.
    if (&a != &b) {
        a = a ^ b; // a now holds the composite difference
        b = a ^ b; // b extracts the original a
        a = a ^ b; // a extracts the original b
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(1)$. Exactly three ALU instructions.
- **Space Complexity:** $O(1)$ auxiliary space.

> [!important]
> **Pointer Aliasing Hazard:** If this swap function is passed two pointers to the exact same element in an array (e.g., `swapWithoutThird(arr[i], arr[i])`), the operation $A \oplus A$ destroys the data, overwriting it with $0$. Always guard the XOR swap with `if (&a != &b)`.
