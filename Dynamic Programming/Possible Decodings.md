# Possible Decodings

## Problem Statement
- a message containing letters from A-Z is encoded to numbers using 'A' -> 1, 'B' -> 2, ..., 'Z' -> 26. Given an encoded message string $s$, determine the total number of ways to decode it.

## Approach / Intuition
- we traverse the string and for each character, we determine how many decodings end at this position. If the current character is valid (1-9), we can form a single character decoding. If the previous character combined with the current forms a valid number (10-26), we can form a two-character decoding. We sum these possibilities using [[Dynamic Programming]], optimizing space just like the [[Fibonacci]] sequence.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$ where $N$ is the length of the string.
- **[[space Complexity]]:** $O(1)$ using two variables.

## Sample Code
```cpp
#include <string>

int numDecodings(std::string s) {
    if (s.empty() || s[0] == '0') return 0;
    int prev2 = 1;
    int prev1 = 1;
    for (int i = 1; i < s.length(); i++) {
        int curr = 0;
        if (s[i] != '0') {
            curr += prev1;
        }
        int twoDigit = (s[i - 1] - '0') * 10 + (s[i] - '0');
        if (twoDigit >= 10 && twoDigit <= 26) {
            curr += prev2;
        }
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

## New Keywords / STL Used
- `std::string`

## Edge Cases
- string starting with '0', continuous '0's.

NEXT: [[Index]]
