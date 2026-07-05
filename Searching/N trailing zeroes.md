# N Trailing Zeroes

## Problem Statement
- given an integer $N$, find the smallest non-negative integer $X$ such that the factorial $X!$ contains exactly $N$ trailing zeroes. If no such integer exists mathematically, return $-1$.


## Approach: Legendre's Formula & Binary Search

- the number of trailing zeroes in $X!$ is entirely dependent on the multiplicity of the prime factor $10$, which breaks down into $2 \times 5$. Since $2$ is far more abundant than $5$ in the prime factorization of a factorial, the number of trailing zeroes is exactly bounded by the multiplicity of $5$.

- by **Legendre's Formula**, the multiplicity of $5$ in $X!$ is:
$$ V_5(X!) = \sum_{i=1}^{\infty} \lfloor \frac{X}{5^i} \rfloor $$
- as $X$ increases, $V_5(X!)$ is strictly non-decreasing. This monotonicity makes it a perfect candidate for **Binary Search**.

- the maximum possible value of $X$ to have $N$ trailing zeroes is bounded by $5N$. So our search space is $L = 0$ to $R = 5N$.
- for midpoint $M$:
   - calculate $Z = V_5(M!)$ using Legendre's Formula.
   - if $Z \ge N$: This is a valid bound. We record $M$ and aggressively search left ($R = M - 1$) to find the *smallest* such integer.
   - if $Z < N$: The integer is too small, search right ($L = M + 1$).
- after finding the smallest $X$ where $V_5(X!) \ge N$, verify if $V_5(X!) == N$. Due to the jumping nature of Legendre's formula, some values of $N$ (like $N=5$) are completely skipped.


## Code Implementation

```cpp
#include <iostream>

using namespace std;

// Calculates trailing zeroes of x! using Legendre's Formula
long long countTrailingZeroes(long long x) {
    long long count = 0;
    while (x > 0) {
        count += x / 5;
        x /= 5;
    }
    return count;
}

long long findSmallestNumWithNZeroes(long long n) {
    // Edge case for zero trailing zeroes
    if (n == 0) return 0;
    
    long long low = 0, high = 5 * n;
    long long ans = -1;
    
    while (low <= high) {
        long long mid = low + (high - low) / 2;
        long long zeroes = countTrailingZeroes(mid);
        
        if (zeroes >= n) {
            ans = mid;
            high = mid - 1; // We want the SMALLEST integer, so push left
        } else {
            low = mid + 1;  // Not enough zeroes, push right
        }
    }
    
    // Verify that the answer yields exactly n zeroes
    if (ans != -1 && countTrailingZeroes(ans) == n) {
        return ans;
    }
    return -1;
}
```


## Complexity Analysis
- **time Complexity:** $O(\log(5N) \cdot \log_5 X)$. The binary search executes $O(\log(5N))$ times, and for each probe, evaluating Legendre's formula takes $O(\log_5 X)$ divisions.
- **space Complexity:** $O(1)$ auxiliary space.

> [!important]
> **Skipped Values:** It's mathematically impossible for any integer's factorial to have exactly $5$ trailing zeroes. $24!$ has $4$ zeroes, but $25!$ has $25/5 + 25/25 = 6$ zeroes. This is why the post-search verification `countTrailingZeroes(ans) == n` is an absolute requirement.

NEXT: [[Index]]
