---
type: concept
tags: [hashing, cpp, math, modular-arithmetic]
date: 2026-07-01
---
# Pair Sums Divisible by k

## Problem Statement
Given an array $A$ of $N$ integers and a modulo modulus $K$, determine if there exists a perfect matching (a partition into $N/2$ pairs) such that the sum of the elements in each pair is strictly congruent to $0 \pmod K$.

---

## Approach: Modular Congruence & Hash Mapping

We analyze the problem strictly within the ring of integers modulo $K$, $\mathbb{Z}_K$.
For a pair $(A_i, A_j)$ to be valid:
$$ (A_i + A_j) \equiv 0 \pmod K $$
This requires their individual residues to satisfy:
$$ (A_i \pmod K + A_j \pmod K) \equiv 0 \pmod K $$
Let $R_i = (A_i \pmod K + K) \pmod K$ to rigorously handle negative dividends in C++.
The requisite condition for pairing is $R_i + R_j = K$, meaning $R_j = K - R_i$.

We project the entire array into a frequency distribution map over the finite domain $[0, K-1]$.
The matching is theoretically possible if and only if the following parity constraints hold:
1. **Zero Residue ($R = 0$):** Elements congruent to $0$ must pair exclusively with other elements congruent to $0$. Thus, the frequency $f(0)$ must be exactly **even**.
2. **Symmetric Residue ($R > 0$):** For any residue $0 < R < K$, the frequency $f(R)$ must exactly equal the conjugate frequency $f(K - R)$.
3. **Midpoint Residue ($R = K/2$):** If $K$ is even, $R = K/2$ is self-conjugate. Similar to $R=0$, elements congruent to $K/2$ must pair with each other. Thus, $f(K/2)$ must be strictly **even**.

---

## Code Implementation

```cpp
#include <vector>
#include <unordered_map>

using namespace std;

bool canArrange(vector<int>& arr, int k) {
    if (arr.size() % 2 != 0) return false; // Topological impossibility

    unordered_map<int, int> remainder_freq;
    
    // Project elements into finite modular field Z_k
    for (int num : arr) {
        int r = ((num % k) + k) % k;
        remainder_freq[r]++;
    }
    
    // Verify symmetric bijection constraints
    for (const auto& [r, count] : remainder_freq) {
        if (r == 0) {
            if (count % 2 != 0) return false;
        } else if (k % 2 == 0 && r == k / 2) {
            if (count % 2 != 0) return false; // Self-conjugate midpoint
        } else {
            if (remainder_freq[r] != remainder_freq[k - r]) return false;
        }
    }
    
    return true;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ amortized. Projecting the array into the hash map takes $O(N)$, and validating the keys takes $O(K)$ bounded by $O(N)$.
- **Space Complexity:** $O(K)$ for the Hash Map bounding the domain of residues.

> [!important]
> **C++ Modulo Mechanics:** The `%` operator in C++ is a *remainder* operator, not a true mathematical modulo. It retains the sign of the dividend. For example, `-5 % 3 = -2`. To map this correctly into the finite ring $\mathbb{Z}_K$, the canonical transformation `((num % k) + k) % k` must be rigorously applied.
