---
type: concept
tags: [bit manipulation, cpp, array]
date: 2026-06-30
---
# Find the Number Occurring an Odd Number of Times

## Problem Statement
Given an array $A$ of positive integers where all numbers occur an even number of times except for one number which occurs an odd number of times, find this unique number.

*Example:* $A = [4, 3, 2, 4, 1, 3, 2, 1, 2]$
*Result:* $2$

---

## Approach: XOR Cumulative Sum (Optimal)

This problem elegantly exploits the mathematical properties of the Bitwise XOR operator ($\oplus$):
1. **Self-Inverse:** $X \oplus X = 0$ (Any number XORed with itself cancels out).
2. **Identity Element:** $X \oplus 0 = X$ (XORing with 0 leaves the number unchanged).
3. **Commutativity & Associativity:** The order of operations does not matter.

If we compute the cumulative XOR sum of all elements in the array:
$$ S = A[0] \oplus A[1] \dots \oplus A[N-1] $$
Because all elements except one appear an even number of times, they will pair up and perfectly cancel out to $0$. The single element appearing an odd number of times will be left unpaired, resulting in $S = \text{target}$.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

int findOdd(const vector<int>& arr) {
    int res = 0;
    
    // XOR all elements together
    for (int x : arr) {
        res ^= x;
    }
    
    return res;
}
```

> [!tip]
> This technique mathematically isolates elements with odd frequencies. If there were *three* elements with odd frequencies, the result would be their XOR sum, not isolating any single one directly.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. We iterate through the array exactly once.
- **Space Complexity:** $O(1)$. We only use a single integer to maintain the XOR sum.
