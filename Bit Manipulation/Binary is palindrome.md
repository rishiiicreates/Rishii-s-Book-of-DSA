---
type: concept
tags: [bit-manipulation, cpp, palindrome, two-pointers, math]
date: 2026-06-30
---
# Binary Palindrome Verification

## Problem Statement
Evaluate whether the 32-bit binary representation of an unsigned integer $N$ is a structural palindrome.

---

## Approach: Two-Pointer Bit Isolation

The binary sequence is structurally bounded by exactly 32 bits. We can execute a mathematical [[Two Pointers]] convergence directly on the bit indices.
1. The left pointer `L` initializes at index $0$ (the LSB).
2. The right pointer `R` initializes at index $31$ (the MSB).
3. To dynamically isolate the $k$-th bit of $N$, we shift the bits right by $k$ and apply a structural `AND` mask: `(N >> k) & 1`.
4. We evaluate the dual extraction at `L` and `R`. If they structurally diverge, the integer is mathematically non-palindromic.
5. Symmetrically advance $L$ and regress $R$ until they converge.

---

## Code Implementation

```cpp
bool isBinaryPalindrome(unsigned int n) {
    int L = 0;
    int R = 31;
    
    while (L < R) {
        // Isolate bits via mathematical shifting and masking
        unsigned int leftBit = (n >> L) & 1;
        unsigned int rightBit = (n >> R) & 1;
        
        // Structural divergence check
        if (leftBit != rightBit) {
            return false;
        }
        
        L++;
        R--;
    }
    
    return true;
}
```

> [!warning]
> The input argument must be strictly typed as `unsigned int`. If $N$ is signed, right-shifting a negative integer triggers arithmetic shifting (padding the MSB with $1$s) rather than logical shifting (padding with $0$s), destroying the integrity of the bit indices.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(1)$. The algorithm strictly performs exactly 16 bitwise validations on a fixed 32-bit integer, mathematically invariant of $N$.
- **Space Complexity:** $\mathcal{O}(1)$. Evaluated strictly via bounded scalar variables.
