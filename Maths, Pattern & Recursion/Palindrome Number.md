---
type: concept
tags: [maths, pattern, recursion, cpp, palindrome]
date: 2026-06-30
---
# Palindrome Number

## Problem Statement
Given an integer $N$, return `true` if $N$ is a palindrome, and `false` otherwise.
A palindrome reads the same forwards and backwards.
*Example:* $N = 121$ is a palindrome. $N = -121$ is not (reads as $121-$).

---

## Approach 1: String Conversion (The Naive Way)

The easiest mental model is to convert the integer to a string, reverse it, and check if it equals the original string.
```cpp
bool isPalindromeString(int x) {
    string str = to_string(x);
    string rev = str;
    reverse(rev.begin(), rev.end());
    return str == rev;
}
```
**Problem:** This requires $O(D)$ auxiliary space to store the string, where $D$ is the number of digits. In strict interviews, you will be asked to do it in $O(1)$ space.

---

## Approach 2: Reversing Half the Number (The Optimal Way)

Instead of reversing the whole number (which could theoretically cause an [[Integer Overflow]]), we can reverse just the *second half* of the number and check if it matches the *first half*.

1. If $N$ is negative, or if it ends in 0 (but isn't exactly 0), it can't be a palindrome.
2. We extract digits from the back and construct a `reversedHalf` number.
3. We know we've reached the middle when the original number $N$ becomes $\le$ `reversedHalf`.

### Code Implementation
```cpp
bool isPalindrome(int x) {
    // Edge Cases: Negative numbers and numbers ending in 0 (e.g. 10, 100)
    if (x < 0 || (x % 10 == 0 && x != 0)) {
        return false;
    }
    
    int reversedHalf = 0;
    
    // We stop when x <= reversedHalf.
    // E.g., for 1221, loop stops when x = 12 and reversedHalf = 12
    while (x > reversedHalf) {
        reversedHalf = (reversedHalf * 10) + (x % 10);
        x /= 10;
    }
    
    // If the length is odd, reversedHalf will have one extra digit.
    // E.g., for 12321, x = 12 and reversedHalf = 123.
    // We drop the middle digit by dividing reversedHalf by 10.
    return x == reversedHalf || x == reversedHalf / 10;
}
```

> [!important]
> The condition `(x % 10 == 0 && x != 0)` is a critical edge case. Without it, the number `10` would result in `x = 1` and `reversedHalf = 0` on the first iteration, and the logic would falsely evaluate.

---

## Complexity Analysis
- **Time Complexity:** $O(\log_{10}(N))$. We only process half the digits of the number.
- **Space Complexity:** $O(1)$. No strings or arrays are allocated.
