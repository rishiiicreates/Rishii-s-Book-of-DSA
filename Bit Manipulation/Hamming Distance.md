---
type: concept
tags: [bit-manipulation, cpp, math, metrics]
date: 2026-07-01
---
# Hamming Distance

## Problem Statement
Given two integers $X$ and $Y$, mathematically compute the Hamming distance $H(X, Y)$, defined as the exact number of bit positions where their binary representations fundamentally differ.

---

## Approach: XOR Isolation and Hamming Weight

The Hamming distance is a metric on the vector space of binary words. It satisfies the metric space axioms: non-negativity, identity of indiscernibles, symmetry, and the triangle inequality.

We compute this using two distinct mathematical transformations:
1. **Divergence Isolation (XOR):** The bitwise XOR operator, $X \oplus Y$, evaluates to $1$ exclusively at the specific bit coordinates where $X_i \ne Y_i$. Thus, the value $Z = X \oplus Y$ forms a binary map of all divergent coordinates.
2. **Hamming Weight (POPCNT):** The Hamming distance $H(X, Y)$ is fundamentally equal to the Hamming weight (count of set bits) of the divergence map $Z$. We isolate and count the set bits in $Z$.

---

## Code Implementation

```cpp
#include <iostream>

using namespace std;

int hammingDistance(int x, int y) {
    // 1. Isolate divergent bits using XOR
    unsigned int z = x ^ y;
    
    // 2. Compute Hamming weight using Brian Kernighan's algorithm
    int distance = 0;
    while (z != 0) {
        z &= (z - 1);
        distance++;
    }
    
    return distance;
    
    /* 
    Alternative Hardware Intrinsic:
    return __builtin_popcount(x ^ y); 
    */
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(K)$, where $K \le 32$ is the number of differing bits. Resolves in $O(1)$ constant time if utilizing hardware `POPCNT` intrinsics.
- **Space Complexity:** $O(1)$ auxiliary space.

> [!tip]
> **Metric Space Implications:** Because $H(X, Y)$ satisfies the triangle inequality $H(X, Y) \le H(X, Z) + H(Z, Y)$, it is heavily utilized in error-correcting codes, cryptography, and nearest-neighbor clustering algorithms operating over boolean hypercubes.
