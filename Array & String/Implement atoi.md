---
type: concept
tags: [array, string, cpp, string-parsing]
date: 2026-06-30
---
# Implement atoi (String to Integer)

## Problem Statement
Implement the `myAtoi(string s)` function, which converts a string to a 32-bit signed integer. 
The algorithm must discard leading whitespaces, handle an optional `+` or `-` sign, parse numerical digits until the first non-digit character, and clamp the integer if it overflows the 32-bit signed integer range $[-2^{31}, 2^{31}-1]$.

*Example:* $S = \text{"   -42 with words"}$
*Result:* $-42$

---

## Approach: Linear Traversal & Boundary Clamping

This problem is a classic string parsing state machine.
1. **Whitespace:** Skip leading spaces.
2. **Sign:** If the next character is `+` or `-`, record the sign multiplier.
3. **Digits:** Process digits one by one. The integer value is accumulated as `ans = ans * 10 + (char - '0')`.
4. **Overflow:** Because $N$ can easily exceed `long long`, we must detect overflow *before* it breaks the 32-bit bounds, or use a `long long` accumulator and clamp it at every step.

---

## Code Implementation

```cpp
#include <string>
#include <climits>
#include <cctype>
using namespace std;

int myAtoi(string s) {
    int i = 0, n = s.length();
    
    // 1. Skip leading whitespaces
    while (i < n && s[i] == ' ') {
        i++;
    }
    
    if (i == n) return 0;
    
    // 2. Determine sign
    int sign = 1;
    if (s[i] == '+' || s[i] == '-') {
        sign = (s[i] == '-') ? -1 : 1;
        i++;
    }
    
    // 3. Accumulate digits
    long long ans = 0;
    while (i < n && isdigit(s[i])) {
        ans = ans * 10 + (s[i] - '0');
        
        // 4. Handle integer overflow dynamically
        if (sign == 1 && ans > INT_MAX) return INT_MAX;
        if (sign == -1 && -ans < INT_MIN) return INT_MIN;
        
        i++;
    }
    
    return sign * ans;
}
```

> [!important]
> Clamping inside the `while` loop is structurally safer. If the string contains 100 consecutive '9's, even a `long long` will overflow. By checking `> INT_MAX` immediately upon appending a digit, we halt and return the boundary safely.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ where $N$ is the length of the string. We iterate through the string linearly.
- **Space Complexity:** $O(1)$ since no additional memory is scaled with the input size.
