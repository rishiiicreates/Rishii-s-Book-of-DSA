---
type: concept
tags: [maths, recursion, cpp, arithmetic]
date: 2026-06-30
---
# Sum of Naturals

## Problem Statement
Calculate the sum of the first $N$ natural numbers.
Mathematically:
$$ \text{Sum} = 1 + 2 + 3 + \dots + N $$

---

## Approach 1: Recursion (The DSA Way)

We can express the sum of $N$ naturals as $N$ plus the sum of $N-1$ naturals.
1. **Base Case:** If $N = 0$, the sum is 0.
2. **Recursive Step:** Return $N + \text{sum}(N-1)$.

### Code Implementation
```cpp
int sumOfNaturals(int n) {
    if (n == 0) return 0;
    return n + sumOfNaturals(n - 1);
}
```

### Complexity
- **Time Complexity:** $O(N)$ — We make exactly $N$ recursive calls.
- **Space Complexity:** $O(N)$ — We place $N$ frames on the recursive Call Stack.

> [!warning] 
> If $N = 10^6$, this recursive approach will likely crash your program with a **Stack Overflow**.

---

## Approach 2: Gauss's Formula (The Best Way)

While recursion is a good mental exercise, the mathematical approach is infinitely superior. The sum of an arithmetic progression can be calculated in a single step using the formula:
$$ \text{Sum} = \frac{N \times (N + 1)}{2} $$

### Code Implementation
```cpp
// We use long long to prevent overflow during the multiplication (N * N)
long long sumOfNaturalsMath(long long n) {
    return n * (n + 1) / 2;
}
```

### Complexity
- **Time Complexity:** $O(1)$ — A single arithmetic operation.
- **Space Complexity:** $O(1)$ — No extra memory is allocated.

> [!important]
> **Always prefer math over loops/recursion.** If a problem can be solved mathematically in $O(1)$ time, it is always the expected optimal solution in competitive programming.
