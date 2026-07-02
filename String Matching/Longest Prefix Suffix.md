---
type: concept
tags: [cpp, string_matching]
date: 2026-06-30
---
# Longest Prefix Suffix

## Problem Statement
Find the length of the longest proper prefix of a string which is also a suffix.

## Approach / Intuition
We build the LPS array used in the [[KMP Algorithm]]. We maintain the length of the previous longest prefix suffix and compare the current character with the character at that length.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <string>
#include <vector>

using namespace std;

vector<int> computeLPSArray(string s) {
    int n = s.length();
    vector<int> lps(n, 0);
    int len = 0;
    int i = 1;
    while (i < n) {
        if (s[i] == s[len]) {
            len++;
            lps[i++] = len;
        } else {
            if (len != 0) {
                len = lps[len - 1];
            } else {
                lps[i++] = 0;
            }
        }
    }
    return lps;
}
```

## New Keywords / STL Used
`std::vector`

## Edge Cases
String of length 1, all characters same.
