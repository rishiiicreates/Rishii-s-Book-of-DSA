---
type: concept
tags: [maths, pattern, cpp, arithmetic]
date: 2026-06-30
---
# Sum of Consecutive

## Problem Statement
Given an integer $N$, count how many ways you can represent $N$ as the sum of consecutive positive integers.
*Example:* $N = 15$. Ways:
1. $15 = 15$
2. $7 + 8 = 15$
3. $4 + 5 + 6 = 15$
4. $1 + 2 + 3 + 4 + 5 = 15$
Total ways: $4$.

---

## Approach: Mathematical Arithmetic Progression

Let $N$ be represented by a sum of $k$ consecutive integers starting from $a$:
$$ N = a + (a+1) + (a+2) + \dots + (a+k-1) $$

Using the sum of an arithmetic progression formula, this becomes:
$$ N = \frac{k}{2} [2a + (k-1)] $$
Multiply by 2 to clear the fraction:
$$ 2N = k(2a + k - 1) $$
Solve for $a$:
$$ a = \frac{\frac{2N}{k} - k + 1}{2} $$

For a valid sequence, $a$ must be a strictly positive integer. 
Therefore, we just iterate over all possible values of $k$ (the length of the sequence). Since $a \ge 1$, $k(k+1)/2 \le N$, meaning we only loop up to $k \approx \sqrt{2N}$.

We check two conditions:
1. $(2N) \pmod k == 0$ (So $2N/k$ is an integer).
2. The resulting numerator $(\frac{2N}{k} - k + 1)$ must be even and strictly greater than $0$ so $a$ is a valid integer.

---

## Code Implementation

```cpp
int consecutiveNumbersSum(int n) {
    int count = 0;
    
    // We loop for sequence lengths k = 1, 2, 3...
    // The maximum possible k happens when a=1, so k(k+1)/2 <= n
    for (long long k = 1; k * (k + 1) <= 2LL * n; k++) {
        
        // Numerator = (2N / k) - k + 1
        // First check if 2N is divisible by k
        if ((2 * n) % k == 0) {
            long long y = (2 * n) / k - k + 1;
            
            // Second check if y is positive and even
            if (y > 0 && y % 2 == 0) {
                count++;
            }
        }
    }
    
    return count;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(\sqrt{N})$. The loop terminates when $k^2 \approx 2N$.
- **Space Complexity:** $O(1)$.
