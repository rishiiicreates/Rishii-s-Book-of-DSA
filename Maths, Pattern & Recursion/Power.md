---
type: concept
tags: [pattern, recursion, cpp, math, exponentiation, binary_exponentiation]
date: 2026-06-30
---
# Power (Binary Exponentiation)

## Problem Statement
Given a base $X$ and an exponent $N$, compute $X^N$.
*Example:* $2^{10} = 1024$.

---

## Approach 1: Naive Loop
The naive approach is to loop $N$ times and multiply $X$ by itself.
```cpp
long long naivePower(int x, int n) {
    long long ans = 1;
    for (int i = 0; i < n; i++) ans *= x;
    return ans;
}
```
**Problem:** If $N = 10^9$, this will take $10^9$ operations and result in a Time Limit Exceeded (TLE). We need an $O(\log N)$ solution.

---

## Approach 2: Binary Exponentiation (Fast Power)

We can drastically reduce the multiplications by halving the exponent at every step.
- If $N$ is **even**: $X^N = (X^{N/2}) \times (X^{N/2})$
- If $N$ is **odd**: $X^N = X \times (X^{N/2}) \times (X^{N/2})$

Instead of computing $X^{N/2}$ twice, we compute it once, store it in a variable, and square it. This drops the time from $O(N)$ down to $O(\log N)$.

### Recursive Implementation

```cpp
long long powerRec(int x, int n) {
    if (n == 0) return 1; // Base case
    
    // Compute once
    long long half = powerRec(x, n / 2);
    
    // If N is even
    if (n % 2 == 0) {
        return half * half;
    } 
    // If N is odd
    else {
        return x * half * half;
    }
}
```

### Iterative Implementation (The Best Way)
The iterative version is preferred in competitive programming because it uses $O(1)$ memory (no Call Stack) and relies on bitwise operations.

```cpp
long long powerIterative(long long x, long long n) {
    long long ans = 1;
    
    // To handle negative exponents (x^-n = 1 / x^n)
    long long nn = n;
    if (nn < 0) nn = -1 * nn;
    
    while (nn > 0) {
        if (nn % 2 == 1) {       // If odd power
            ans = ans * x;       // Multiply current base to ans
            nn = nn - 1;         // Make it even
        } else {                 // If even power
            x = x * x;           // Square the base
            nn = nn / 2;         // Halve the exponent
        }
    }
    
    if (n < 0) return (double)1.0 / (double)ans;
    return ans;
}
```

> [!important]
> **Modulo Arithmetic:** Problems usually ask you to return the answer modulo $10^9 + 7$ to prevent overflow. Simply apply `% MOD` after every single multiplication inside the `powerIterative` function.

---

## Complexity Analysis
- **Time Complexity:** $O(\log N)$. We halve the exponent at every step.
- **Space Complexity:** $O(\log N)$ for the recursive approach due to the Call Stack, and **$O(1)$** for the iterative approach.
