---
type: concept
tags: [maths, pattern, cpp, armstrong]
date: 2026-06-30
---
# Armstrong Number

## Problem Statement
Given an integer $N$, determine if it is an Armstrong number.
An Armstrong number (also known as a Narcissistic number) is a number that is equal to the sum of its own digits, each raised to the power of the *number of digits*.

*Example:* $N = 153$. There are 3 digits.
$1^3 + 5^3 + 3^3 = 1 + 125 + 27 = 153$. It is an Armstrong number.

---

## Approach: Digit Extraction

1. **Count the digits:** First, we need to know the power $K$ to raise each digit to. We can use the logarithmic $O(1)$ approach from [[Count Digits]].
2. **Extract digits:** We can extract the last digit of a number using `N % 10`.
3. **Truncate:** We remove the last digit from the number using `N / 10`.
4. **Sum and Compare:** We accumulate $digit^K$ into a sum and compare it to the original number.

---

## Code Implementation

```cpp
#include <iostream>
#include <cmath>
using namespace std;

bool isArmstrong(int n) {
    if (n < 0) return false; // Armstrong numbers are positive
    if (n == 0) return true;
    
    int original = n;
    long long sum = 0;
    
    // Step 1: Find the number of digits (K)
    int k = floor(log10(n)) + 1;
    
    // Step 2 & 3: Extract and accumulate
    while (n > 0) {
        int digit = n % 10;
        sum += pow(digit, k);
        n /= 10;
    }
    
    // Step 4: Compare
    return sum == original;
}
```

> [!warning]
> The `pow()` function in C++ returns a `double` (floating-point number). When dealing with large precise integers, `pow()` can suffer from floating-point precision loss. If a problem has massive inputs, you should write a custom integer-based power function using [[Binary Exponentiation]].

---

## Complexity Analysis
- **Time Complexity:** $O(\log_{10}(N))$. The `while` loop runs exactly $K$ times, where $K$ is the number of digits.
- **Space Complexity:** $O(1)$. We only use a few integer variables.
