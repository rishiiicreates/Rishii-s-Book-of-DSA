---
type: concept
tags: [maths, pattern, cpp, rounding]
date: 2026-06-30
---
# Closest Number

## Problem Statement
Given two integers $N$ and $M$ ($M \neq 0$), find the number closest to $N$ that is perfectly divisible by $M$. If there are two equally close numbers, return the one with the maximum absolute value.

*Example:* $N = 13, M = 4$. Multiples of 4 are $8, 12, 16$. The closest to 13 is $12$.

---

## Approach: Mathematical Quotients

Instead of looping forwards and backwards from $N$, we can use integer division logic.
When you divide $N$ by $M$ using integer division (`q = N / M`), you find the closest multiple of $M$ that is *less than or equal* to $N$ (e.g., $13 / 4 = 3$, and $3 \times 4 = 12$).

The only other candidate for the closest number is the *next* multiple of $M$, which is $(q \times M) + M$ if $N \times M > 0$, or $(q \times M) - M$ otherwise.

1. Find the quotient $q = N / M$.
2. Find the floor multiple $n1 = M \times q$.
3. Find the next multiple $n2$. If $N \times M > 0$, $n2 = M \times (q + 1)$. Else $n2 = M \times (q - 1)$.
4. Compare the absolute differences $|N - n1|$ and $|N - n2|$ and return the closer one.

---

## Code Implementation

```cpp
#include <iostream>
#include <cmath>
using namespace std;

int closestNumber(int n, int m) {
    int q = n / m;
    
    // Candidate 1: the floor multiple
    int n1 = m * q;
    
    // Candidate 2: the next multiple
    int n2;
    if ((n * m) > 0) { // Same sign
        n2 = m * (q + 1);
    } else {           // Different signs
        n2 = m * (q - 1);
    }
    
    // Compare distances
    if (abs(n - n1) < abs(n - n2)) {
        return n1;
    } 
    return n2; 
    // Notice if they are equal, returning n2 satisfies the "maximum absolute value" rule
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(1)$. Pure arithmetic operations.
- **Space Complexity:** $O(1)$.
