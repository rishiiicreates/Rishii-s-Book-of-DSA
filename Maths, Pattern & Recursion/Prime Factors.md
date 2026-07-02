---
type: concept
tags: [maths, pattern, cpp, prime_factors]
date: 2026-06-30
---
# Prime Factors

## Problem Statement
Given an integer $N$, find all of its prime factors.
*Example:* For $N = 315$, the prime factorization is $3 \times 3 \times 5 \times 7$. The prime factors are `[3, 3, 5, 7]`.

---

## Approach: Mathematical Factorization

The naive approach is to loop from $2$ to $N$ and divide. But just like [[Prime Testing]], we can heavily optimize this by realizing we only need to search up to $\sqrt{N}$.

**The Algorithm:**
1. **Strip all 2s:** Even numbers are common. Divide $N$ by 2 as long as it's even, storing `2` each time. This makes $N$ entirely odd.
2. **Search up to $\sqrt{N}$:** Now loop from $i = 3$ up to $\sqrt{N}$, incrementing by 2 (checking only odd numbers: 3, 5, 7...).
3. **Divide greedily:** While $N$ is divisible by $i$, push $i$ to our list and divide $N$ by $i$. Because we divide greedily, we will never accidentally push a composite number (like 9), because its prime factors (like 3) were already completely stripped out!
4. **The final prime:** If after the loop, $N$ is still greater than 2, then $N$ itself is a massive prime number (the single factor that was greater than $\sqrt{N}$). We push it to the list.

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>
using namespace std;

vector<int> primeFactors(int n) {
    vector<int> factors;
    
    // Step 1: Strip all 2s
    while (n % 2 == 0) {
        factors.push_back(2);
        n /= 2;
    }
    
    // Step 2 & 3: Check odd numbers up to sqrt(N)
    for (int i = 3; i * i <= n; i += 2) {
        while (n % i == 0) {
            factors.push_back(i);
            n /= i; // Shrink N
        }
    }
    
    // Step 4: Handle the remaining prime (if any)
    if (n > 2) {
        factors.push_back(n);
    }
    
    return factors;
}
```

> [!tip]
> Why does `if (n > 2)` work at the end? A number can have at most **one** prime factor strictly greater than its square root. If after stripping everything up to $\sqrt{N}$ the remaining value is not $1$, it must be that single massive prime factor.

---

## Complexity Analysis
- **Time Complexity:** $O(\sqrt{N})$. The loop goes up to $\sqrt{N}$. The inner `while` loop runs at most $O(\log N)$ times overall, making the dominant time $O(\sqrt{N})$.
- **Space Complexity:** $O(\log N)$ to store the array of factors. A number $N$ can have at most $\approx \log_2(N)$ prime factors (in the worst case where it's a power of 2).
