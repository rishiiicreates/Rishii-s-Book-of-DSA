# Prime Testing

## Problem Statement
- given an integer $N$, determine whether it is a prime number.
- a number is prime if it has exactly two distinct positive divisors: 1 and itself.


## Approach: The $\sqrt{N}$ Optimization

- the naive approach is to loop from $2$ to $N-1$ and check if `N % i == 0`. This takes $O(N)$ time, which will result in Time Limit Exceeded (TLE) if $N \approx 10^9$.

- **the Mathematical Trick:** Divisors always exist in pairs. If $N$ is divisible by $A$, it must also be divisible by $B$ such that $A \times B = N$.
- if both $A$ and $B$ were greater than $\sqrt{N}$, then $A \times B > N$, which is impossible. Therefore, at least one of the divisors must be $\le \sqrt{N}$.

- if we don't find any divisors up to $\sqrt{N}$, we are mathematically guaranteed that $N$ is prime!


## Code Implementation

```cpp
bool isPrime(int n) {
    if (n <= 1) return false; // 0, 1, and negative numbers are not prime
    
    // Check up to the square root of n
    // We use i * i <= n instead of i <= sqrt(n) because calculating 
    // sqrt is a slow floating-point operation.
    for (long long i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            return false;
        }
    }
    return true;
}
```

> [!important]
> Notice the use of `long long i` in the loop condition `i * i <= n`. If `n` is exactly `INT_MAX`, `i * i` will overflow a 32-bit integer, resulting in a negative number and creating an infinite loop. Always use `long long` for squared loop bounds!


## Further Optimization ($O(\sqrt{N}) / 3$)
- you can skip checking all even numbers and all multiples of 3 to speed up the loop by a massive constant factor:

```cpp
bool isPrimeOptimized(int n) {
    if (n <= 1) return false;
    if (n == 2 || n == 3) return true;
    if (n % 2 == 0 || n % 3 == 0) return false;
    
    for (long long i = 5; i * i <= n; i += 6) {
        if (n % i == 0 || n % (i + 2) == 0) {
            return false;
        }
    }
    return true;
}
```


## Complexity Analysis
- **time Complexity:** $O(\sqrt{N})$. The loop only runs up to the square root of the number.
- **space Complexity:** $O(1)$. No extra memory is used.

NEXT: [[Index]]
