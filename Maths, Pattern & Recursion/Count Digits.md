---
type: concept
tags: [maths, pattern, cpp, count_digits]
date: 2026-06-30
---
# Count Digits

## Problem Statement
Given an integer $N$, count the total number of digits it contains.
*Example:* For $N = 7352$, the answer is $4$.

---

## Approach 1: Division Loop ($O(d)$ Time)

The most standard programming approach is to continuously divide the number by 10. Every division drops the last digit (e.g., $7352 / 10 = 735$). We increment a counter until the number hits 0.

```cpp
int countDigitsLoop(long long n) {
    if (n == 0) return 1; // Edge case
    
    n = abs(n); // Handle negative numbers
    int count = 0;
    while (n > 0) {
        count++;
        n /= 10;
    }
    return count;
}
```
**Complexity:** $O(d)$ time and $O(1)$ space, where $d$ is the number of digits. Since $d = \log_{10}(N)$, the time complexity is technically $O(\log_{10} N)$.

---

## Approach 2: String Conversion (The Easy Way)

If performance isn't aggressively strict, you can convert the number to a string and simply ask for its length.

```cpp
#include <string>

int countDigitsString(long long n) {
    if (n < 0) n = -n; // Alternatively: return to_string(n).length() - 1;
    return to_string(n).length();
}
```
**Complexity:** $O(d)$ time to convert, but $O(d)$ space to store the string in memory.

---

## Approach 3: Logarithmic Math (The Best Way)

Mathematics provides a constant-time $O(1)$ solution. 
The base-10 logarithm of a number tells you what power you must raise 10 to in order to get that number. 
For $N = 7352$, $\log_{10}(7352) \approx 3.866$. 
If we take the floor of this number (which truncates it to $3$) and add $1$, we get $4$ digits.

Formula:
$$ \text{Digits} = \lfloor \log_{10}(N) \rfloor + 1 $$

```cpp
#include <cmath>

int countDigitsMath(long long n) {
    if (n == 0) return 1;
    return floor(log10(abs(n))) + 1;
}
```

> [!important]
> **Performance:** The mathematical $O(1)$ approach is the absolute fastest way to compute this in competitive programming. Just be careful to `#include <cmath>` and handle $N=0$ explicitly, since $\log_{10}(0)$ is mathematically undefined (returns `-Infinity`).
