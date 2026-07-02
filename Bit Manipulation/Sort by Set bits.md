---
type: concept
tags: [bit-manipulation, cpp, sorting, custom-comparator, math]
date: 2026-06-30
---
# Sort by Set Bits (Hamming Weight Sorting)

## Problem Statement
Given an array of integers, sort them in descending order based on their Hamming weight (the mathematical count of set bits, or $1$s, in their binary representation). If two integers possess the exact same Hamming weight, their original relative temporal order must be strictly preserved.

---

## Approach: Stable Sorting with Compiler Intrinsics

To execute this, we must define a strict weak ordering mathematical relation for a [[Custom Comparator]].
1. The primary sorting axis is the Hamming weight: $H(A) > H(B)$.
2. The secondary sorting axis mathematically requires stability. Therefore, we must mandate the use of `std::stable_sort` instead of `std::sort`. `std::sort` (Introsort) is inherently unstable and will destructively mutate the relative ordering of structurally equivalent elements.

To evaluate $H(x)$ efficiently, we utilize the hardware-accelerated GCC compiler intrinsic `__builtin_popcount` (or `__builtin_popcountll` for 64-bit integers), which maps directly to the CPU's `POPCNT` instruction, evaluating the set bits in $\mathcal{O}(1)$ hardware cycles.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

void sortBySetBitCount(vector<int>& arr) {
    // stable_sort is a strict mathematical requirement for relative order preservation
    stable_sort(arr.begin(), arr.end(), [](const int a, const int b) {
        // Evaluate the Hamming weights via hardware intrinsic
        int countA = __builtin_popcount(a);
        int countB = __builtin_popcount(b);
        
        // Strict Weak Ordering: Descending
        return countA > countB;
    });
}
```

> [!important]
> If you are processing `long long` integers, you must strictly use `__builtin_popcountll`. Using the standard 32-bit `__builtin_popcount` on a 64-bit integer truncates the upper 32 bits, resulting in catastrophic mathematical failures during comparison.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N \log N)$. `std::stable_sort` mandates $N \log N$ comparisons, and the hardware-accelerated intrinsic ensures each comparison strictly evaluates in $\mathcal{O}(1)$ time.
- **Space Complexity:** $\mathcal{O}(N)$. `std::stable_sort` mathematically requires $\mathcal{O}(N)$ auxiliary memory space to perform its stable merge operations.
