---
type: concept
tags: [bit-manipulation, cpp, math, combinatorics]
date: 2026-07-01
---
# Sum of Subset XORs

## Problem Statement
Given an array $A$ of $N$ integers, compute the mathematical sum of the XOR totals across all $2^N$ possible subsets.

---

## Approach: Probabilistic Bit Superposition

A brute-force traversal of the $2^N$ power set takes $O(N \cdot 2^N)$ time. We can mathematically compress this to $O(N)$.
We decouple the problem orthogonally by analyzing the contribution of the $k$-th bit.

Consider the $k$-th bit column across the array $A$.
- **Case 1: The $k$-th bit is `0` in ALL elements.**
  In this case, no subset can possibly have the $k$-th bit set. Its total combinatorial contribution is $0$.
- **Case 2: The $k$-th bit is `1` in AT LEAST ONE element.**
  If there is at least one element with the $k$-th bit set, then exactly half of the $2^N$ subsets will have an odd number of `1`s at the $k$-th position (thus XORing to `1`), and the other half will have an even number (XORing to `0`).
  Therefore, exactly $2^{N-1}$ subsets will have the $k$-th bit set.
  The positional contribution of the $k$-th bit is $2^k \times 2^{N-1}$.

Summing this logic over all 32 bits:
The bitwise OR operator $A_0 \lor A_1 \dots \lor A_{N-1}$ creates a mask of all bits that fall into Case 2.
Thus, the final sum is simply:
$$ \text{Total} = (A_0 \lor A_1 \dots \lor A_{N-1}) \times 2^{N-1} $$

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

long long subsetXORSum(vector<int>& nums) {
    int or_sum = 0;
    int n = nums.size();
    
    // Collapse to global superposition state
    for (int num : nums) {
        or_sum |= num;
    }
    
    // Multiply by 2^(N-1). Use long long to avoid shift overflow
    return (long long)or_sum * (1LL << (n - 1));
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ strict linear scan to collapse the OR superposition.
- **Space Complexity:** $O(1)$ auxiliary space.

> [!important]
> **Exponential Shift Overflow:** The multiplier $2^{N-1}$ grows exponentially. For $N > 62$, `1LL << (N - 1)` triggers undefined behavior on 64-bit architectures, and the total sum exceeds 64-bit numerical limits. Modular arithmetic is fundamentally required if bounds scale beyond $N=60$.
