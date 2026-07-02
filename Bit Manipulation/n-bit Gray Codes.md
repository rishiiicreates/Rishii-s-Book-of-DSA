---
type: concept
tags: [bit-manipulation, cpp, math, combinatorics]
date: 2026-07-01
---
# n-bit Gray Codes

## Problem Statement
Generate the complete combinatorial sequence of $n$-bit Gray codes. A Gray code sequence is an ordering of the $2^n$ binary numbers such that any two successive values differ by exactly one bit (Hamming distance $= 1$).

---

## Approach: Mathematical Bijection

A naive recursive generation via string reflection takes $O(n \cdot 2^n)$ time. We can achieve a strict $O(2^n)$ algorithmic complexity by exploiting a direct mathematical bijection between the standard binary sequence and the Gray code sequence.

Let $G(i)$ denote the $i$-th Gray code. The bijection maps the standard binary integer $i$ directly to $G(i)$:
$$ G(i) = i \oplus \lfloor i / 2 \rfloor \equiv i \oplus (i \gg 1) $$

**Mathematical Proof of Adjacency:**
Consider $i$ and $i+1$. In standard binary addition, $i+1$ flips the lowest unset bit of $i$, and all bits below it. Let this flipped bit be at position $k$. 
Thus, bits below $k$ undergo a total parity inversion.
When computing $(i+1) \oplus ((i+1) \gg 1)$, the cascading XOR operation perfectly cancels out the parity inversions for all bits below $k$.
The only bit that ultimately diverges between $G(i)$ and $G(i+1)$ is exactly the bit at position $k$. Thus, the Hamming distance is strictly $1$.

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

vector<int> grayCode(int n) {
    // Generate exactly 2^n states
    int limit = 1 << n;
    vector<int> res(limit);
    
    // Direct bijective mapping for each state
    for (int i = 0; i < limit; i++) {
        res[i] = i ^ (i >> 1);
    }
    
    return res;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(2^n)$. Exact limit of states in the vector space.
- **Space Complexity:** $O(2^n)$ auxiliary space to store the mathematical sequence.

> [!important]
> **Hamiltonian Cycle:** The standard Gray Code forms a Hamiltonian Cycle on an $n$-dimensional Hypercube graph. Since $G(2^n - 1)$ and $G(0)$ also differ by exactly one bit, the path wraps around perfectly, proving structural completeness.
