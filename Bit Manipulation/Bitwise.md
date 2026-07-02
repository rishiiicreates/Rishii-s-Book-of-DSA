---
type: concept
tags: [bit-manipulation, cpp, math, boolean-algebra]
date: 2026-07-01
---
# Bitwise Basics

## Problem Statement
Define and evaluate the fundamental bitwise operations (AND, OR, XOR, NOT, Left Shift, Right Shift) as transformations acting upon the Boolean ring structure of integer representations.

---

## Approach: Boolean Algebra & Binary Representation

Integers in modern architectures are represented as finite-length arrays of bits $\vec{b} = \langle b_{n-1}, b_{n-2}, \dots, b_0 \rangle$, where $b_i \in \{0, 1\}$. 
Bitwise operators apply Boolean algebraic operations element-wise parallelly across the bit vector.

1. **AND (`&`)**: $c_i = a_i \land b_i$. Used heavily for **Masking** (extracting specific bits by zeroing out others).
2. **OR (`|`)**: $c_i = a_i \lor b_i$. Used for **Setting** specific bits.
3. **XOR (`^`)**: $c_i = a_i \oplus b_i$. Represents modulo-2 addition. Critical properties: 
   - Nilpotence: $A \oplus A = 0$
   - Identity: $A \oplus 0 = A$
   - Commutativity/Associativity: $A \oplus B = B \oplus A$
4. **NOT (`~`)**: $c_i = \lnot a_i$. Computes the **One's Complement**. By Two's Complement representation, mathematically $\sim A = -(A + 1)$.
5. **Left Shift (`<<`)**: $\vec{c} = \vec{a} \text{ shifted left by } k$. Mathematically equivalent to multiplying by $2^k$: $A \ll k \equiv A \times 2^k$.
6. **Right Shift (`>>`)**: $\vec{c} = \vec{a} \text{ shifted right by } k$. Mathematically equivalent to integer division by $2^k$: $A \gg k \equiv \lfloor A / 2^k \rfloor$.

---

## Code Implementation

```cpp
#include <iostream>

using namespace std;

void bitwiseBasics(int a, int b) {
    int bit_and = a & b;       // Intersection of set bits
    int bit_or  = a | b;       // Union of set bits
    int bit_xor = a ^ b;       // Symmetric difference
    
    int bit_not = ~a;          // Mathematical reflection: -(a + 1)
    
    // Arithmetic operations via bit shifting
    int left_shift  = a << 1;  // a * 2 (Beware of overflow)
    int right_shift = a >> 1;  // floor(a / 2)
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(1)$. Hardware ALU natively executes these in single CPU clock cycles.
- **Space Complexity:** $O(1)$ auxiliary space.

> [!warning]
> **Shift Overflow & Undefined Behavior (UB):** In C++, shifting a 32-bit integer by $k \ge 32$ or $k < 0$ is strictly Undefined Behavior. Furthermore, left-shifting a negative signed integer is theoretically UB, though most compilers handle it via arithmetic shifts. Always use `unsigned` types when performing heavy bit manipulations.
