---
type: concept
tags: [bit-manipulation, cpp, math, bit-masking]
date: 2026-06-30
---
# Power of 4 Identification

## Problem Statement
Given a signed 32-bit integer $N$, mathematically determine whether it is an exact power of $4$. (i.e., $N == 4^k$ for some integer $k \ge 0$).

---

## Approach: Structural Bitmask Constraints

A number is a mathematically valid power of $4$ if and only if it satisfies three strict structural constraints simultaneously:
1. **Positivity:** $N > 0$. Negative numbers and $0$ are structurally invalid.
2. **Singular Set Bit:** $N$ must be a power of $2$. A power of $2$ contains exactly one `1` in its binary representation. This is validated by $N \ \& \ (N - 1) == 0$.
3. **Even Bit Position:** In a 0-indexed binary representation, powers of $4$ exclusively populate the even indices:
   - $4^0 = 1 = (00000001)_2 \implies$ bit 0
   - $4^1 = 4 = (00000100)_2 \implies$ bit 2
   - $4^2 = 16 = (00010000)_2 \implies$ bit 4
   
   To validate this, we construct a 32-bit mask where strictly all even indices are set to $1$. In hexadecimal, this is `0x55555555` (since $5 = (0101)_2$). If $N \ \& \ \text{0x55555555}$ is non-zero, the singular bit mathematically resides on an even index.

---

## Code Implementation

```cpp
bool isPowerOfFour(int n) {
    // Constraint 1: Strict positivity
    if (n <= 0) return false;
    
    // Constraint 2: Power of 2 (Singular Bit Isolation)
    bool isPowerOfTwo = (n & (n - 1)) == 0;
    
    // Constraint 3: Positional validity via Even-Bit Mask
    bool isEvenPosition = (n & 0x55555555) != 0;
    
    return isPowerOfTwo && isEvenPosition;
}
```

> [!tip]
> A mathematically alternative (but slightly slower) check for Constraint 3 uses modulo arithmetic. Since $4 \equiv 1 \pmod 3$, any power $4^k \equiv 1^k \equiv 1 \pmod 3$. Therefore, `(n - 1) % 3 == 0` is a valid mathematical substitute for the bitmask `0x55555555`.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(1)$. Evaluated strictly via bounded CPU bitwise instructions.
- **Space Complexity:** $\mathcal{O}(1)$. Evaluates purely in-place on CPU registers.
