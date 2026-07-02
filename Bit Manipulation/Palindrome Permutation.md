---
type: concept
tags: [bit manipulation, cpp, string-manipulation]
date: 2026-06-30
---
# Palindrome Permutation

## Problem Statement
Given a string $S$, determine if the characters can be rearranged (permuted) to form a palindrome.

*Example:* $S = \text{"code"}$
*Result:* `false`

*Example:* $S = \text{"aab"}$
*Result:* `true` (can form "aba")

---

## Approach: Bitwise Toggle Mapping

A string can form a palindrome if and only if at most one character occurs an odd number of times (the center character). All other characters must occur an even number of times to balance both sides.

Instead of a hash map to count frequencies, we can use an integer as a bit vector to track parity (even/odd).
1. We have 26 lowercase English letters. An integer (which is at least 32 bits) easily accommodates this.
2. For each character $C$ in the string, map it to a bit index $i = C - \text{'a'}$.
3. Toggle that bit in our integer using XOR: `bitVector ^= (1 << i)`.
   - If a character appears twice, the bit toggles on then off (even parity).
   - If a character appears three times, the bit toggles on, off, then on (odd parity).
4. After processing the string, we mathematically check if at most one bit is set. A number $X$ has at most one bit set if and only if $X \ \& \ (X - 1) == 0$.

---

## Code Implementation

```cpp
#include <string>
using namespace std;

bool canPermutePalindrome(const string& s) {
    int bitVector = 0;
    
    // Toggle parity for each character
    for (char c : s) {
        int mask = 1 << (c - 'a');
        bitVector ^= mask;
    }
    
    // Check if at most one bit is set
    return (bitVector & (bitVector - 1)) == 0;
}
```

> [!tip]
> The operation `X & (X - 1)` clears the rightmost set bit of $X$. If $X$ only had one bit set to begin with (or was already $0$), clearing it results in exactly $0$.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ where $N$ is the length of the string. We do a single pass over the string.
- **Space Complexity:** $O(1)$. We only use a single 32-bit integer.
