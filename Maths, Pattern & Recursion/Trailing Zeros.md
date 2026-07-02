---
type: concept
tags: [maths, pattern, cpp, trailing-zeros]
date: 2026-06-30
---
# Trailing Zeros in Factorial

## Problem Statement
Given an integer $N$, find the number of trailing zeros in $N!$ (N factorial).
*Example:* $5! = 120$ (1 trailing zero). $10! = 3628800$ (2 trailing zeros).

---

## Approach: Prime Factorization (Legendre's Formula)

You **cannot** simply calculate $N!$ and count the zeros, because factorials grow incredibly fast and will cause integer overflow for even small inputs like $21!$.

**The Mathematical Trick:** 
A trailing zero is created every time we multiply by $10$. 
The prime factors of $10$ are $2$ and $5$. Therefore, the number of trailing zeros is exactly equal to the number of $(2, 5)$ pairs in the prime factorization of $N!$.

In any factorial, there are wildly more multiples of $2$ than multiples of $5$. Therefore, the bottleneck is always the number of $5$s. 
To find the answer, we simply need to count **how many times 5 appears in the prime factors** of all numbers from $1$ to $N$.

### Counting the 5s
- Every multiple of $5$ contributes one 5.
- Every multiple of $25$ contributes an *extra* 5.
- Every multiple of $125$ contributes an *extra* 5... and so on.

Formula:
$$ \text{Zeros} = \lfloor \frac{N}{5} \rfloor + \lfloor \frac{N}{25} \rfloor + \lfloor \frac{N}{125} \rfloor + \dots $$

---

## Code Implementation

```cpp
int trailingZeroes(int n) {
    int count = 0;
    
    // We use long long for 'i' to prevent overflow when i *= 5
    for (long long i = 5; n / i >= 1; i *= 5) {
        count += n / i;
    }
    
    return count;
}
```

> [!tip]
> A common interview follow-up is asking why we use a `while` loop or `for` loop with powers of 5 instead of iterating $1 \dots N$. By iterating over powers of 5 ($5, 25, 125\dots$), we instantly jump to the answers and completely avoid processing individual numbers!

---

## Complexity Analysis
- **Time Complexity:** $O(\log_5(N))$. The loop variable `i` multiplies by 5 every iteration, drastically reducing the search space.
- **Space Complexity:** $O(1)$. No extra memory is allocated.
