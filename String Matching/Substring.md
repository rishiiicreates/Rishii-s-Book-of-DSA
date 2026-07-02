---
type: concept
tags: [cpp, string_matching]
date: 2026-06-30
---
# Substring

## Problem Statement
Find if a string $P$ (pattern) is a substring of string $T$ (text).

## Approach / Intuition
A naive [[String Matching]] approach compares the pattern against all possible starting positions in the text. We slide the pattern over the text one by one and check for a match.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N \times M)$
- **[[Space Complexity]]:** $O(1)$

## Sample Code
```cpp
#include <string>

using namespace std;

int findSubstring(string text, string pattern) {
    int n = text.length();
    int m = pattern.length();
    for (int i = 0; i <= n - m; i++) {
        int j;
        for (j = 0; j < m; j++) {
            if (text[i + j] != pattern[j])
                break;
        }
        if (j == m) return i;
    }
    return -1;
}
```

## New Keywords / STL Used
`std::string`

## Edge Cases
Empty pattern, empty text, pattern longer than text.
