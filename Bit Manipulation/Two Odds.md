---
type: concept
tags: [bit manipulation, cpp, array]
date: 2026-06-30
---
# Find Two Numbers Occurring an Odd Number of Times

## Problem Statement
Given an array $A$ in which all numbers occur an even number of times except two distinct numbers $X$ and $Y$ that occur an odd number of times, find $X$ and $Y$.

*Example:* $A = [4, 2, 4, 5, 2, 3, 3, 1]$
*Result:* $5, 1$

---

## Approach: XOR and Rightmost Set Bit Isolation

We build upon the logic used to find a single odd-occurring number.

1. **Total XOR:** Compute the XOR sum of the entire array. Since all even-frequency numbers cancel out, the result is exactly $S = X \oplus Y$.
2. **Bit Isolation:** Because $X$ and $Y$ are distinct, they must differ in at least one bit. Thus, $S \neq 0$, and $S$ will have at least one set bit (`1`). We can mathematically isolate the rightmost set bit using the two's complement trick: `mask = S & ~(S - 1)` or more commonly `mask = S & (-S)`.
3. **Partitioning:** This `mask` represents a specific bit position where $X$ and $Y$ differ (one has a `1`, the other has a `0`). We partition the array $A$ into two mathematical sets:
   - Set 1: Elements where `(A[i] & mask) != 0`
   - Set 2: Elements where `(A[i] & mask) == 0`
4. **Resolution:** Since $X$ and $Y$ fall into different sets, and all other numbers appear in pairs (and thus go into the same set as their pair), XORing the elements within Set 1 yields $X$, and XORing the elements within Set 2 yields $Y$.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

vector<int> findTwoOdds(const vector<int>& arr) {
    // 1. Find X ^ Y
    int xorSum = 0;
    for (int x : arr) {
        xorSum ^= x;
    }
    
    // 2. Isolate the rightmost set bit
    // Note: Cast to unsigned to prevent overflow behavior on INT_MIN in C++
    unsigned int rightmostSetBit = xorSum & -(unsigned int)xorSum;
    
    // 3. Partition and resolve
    int num1 = 0;
    int num2 = 0;
    for (int x : arr) {
        if (x & rightmostSetBit) {
            num1 ^= x; // Falls into Set 1
        } else {
            num2 ^= x; // Falls into Set 2
        }
    }
    
    return {num1, num2};
}
```

> [!important]
> The expression `x & -x` is a beautiful fundamental bitwise trick. It works because in two's complement, `-x` is `~x + 1`. Adding $1$ to the inverted bits flips everything up to the first $0$ (which was the rightmost $1$ originally), effectively isolating that exact bit when ANDed with the original number.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. We traverse the array exactly twice.
- **Space Complexity:** $O(1)$. Operations are performed in-place.
