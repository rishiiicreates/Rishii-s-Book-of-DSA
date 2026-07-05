# Palindrome Permutation

## Problem Statement
- given a string $S$, determine if the characters can be rearranged (permuted) to form a palindrome.

- *example:* $S = \text{"code"}$
- *result:* `false`

- *example:* $S = \text{"aab"}$
- *result:* `true` (can form "aba")


## Approach: Bitwise Toggle Mapping

- a string can form a palindrome if and only if at most one character occurs an odd number of times (the center character). All other characters must occur an even number of times to balance both sides.

- instead of a hash map to count frequencies, we can use an integer as a bit vector to track parity (even/odd).
- we have 26 lowercase English letters. An integer (which is at least 32 bits) easily accommodates this.
- for each character $C$ in the string, map it to a bit index $i = C - \text{'a'}$.
- toggle that bit in our integer using XOR: `bitVector ^= (1 << i)`.
   - if a character appears twice, the bit toggles on then off (even parity).
   - if a character appears three times, the bit toggles on, off, then on (odd parity).
- after processing the string, we mathematically check if at most one bit is set. A number $X$ has at most one bit set if and only if $X \ \& \ (X - 1) == 0$.


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


## Complexity Analysis
- **time Complexity:** $O(N)$ where $N$ is the length of the string. We do a single pass over the string.
- **space Complexity:** $O(1)$. We only use a single 32-bit integer.

NEXT: [[Index]]
