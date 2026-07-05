# Sum of Naturals

## Problem Statement
- calculate the sum of the first $N$ natural numbers.
- mathematically:
$$ \text{Sum} = 1 + 2 + 3 + \dots + N $$


## Approach 1: Recursion (The DSA Way)

- we can express the sum of $N$ naturals as $N$ plus the sum of $N-1$ naturals.
- **base Case:** If $N = 0$, the sum is 0.
- **recursive Step:** Return $N + \text{sum}(N-1)$.

### Code Implementation
```cpp
int sumOfNaturals(int n) {
    if (n == 0) return 0;
    return n + sumOfNaturals(n - 1);
}
```

### Complexity
- **time Complexity:** $O(N)$ — We make exactly $N$ recursive calls.
- **space Complexity:** $O(N)$ — We place $N$ frames on the recursive Call Stack.

> [!warning] 
> If $N = 10^6$, this recursive approach will likely crash your program with a **Stack Overflow**.


## Approach 2: Gauss's Formula (The Best Way)

- while recursion is a good mental exercise, the mathematical approach is infinitely superior. The sum of an arithmetic progression can be calculated in a single step using the formula:
$$ \text{Sum} = \frac{N \times (N + 1)}{2} $$

### Code Implementation
```cpp
// We use long long to prevent overflow during the multiplication (N * N)
long long sumOfNaturalsMath(long long n) {
    return n * (n + 1) / 2;
}
```

### Complexity
- **time Complexity:** $O(1)$ — A single arithmetic operation.
- **space Complexity:** $O(1)$ — No extra memory is allocated.

> [!important]
> **Always prefer math over loops/recursion.** If a problem can be solved mathematically in $O(1)$ time, it is always the expected optimal solution in competitive programming.

NEXT: [[Index]]
