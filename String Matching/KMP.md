# KMP

## Problem Statement
- efficiently find all occurrences of a pattern in a text.

## Approach / Intuition
- the [[KMP Algorithm]] precomputes a [[Longest Prefix Suffix]] (LPS) array for the pattern. This tells us the maximum length of a matched prefix that is also a suffix, allowing us to skip redundant comparisons when a mismatch occurs.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N + M)$
- **[[space Complexity]]:** $O(M)$

## Sample Code
```cpp
#include <string>
#include <vector>

using namespace std;

vector<int> kmp(string text, string pattern) {
    int n = text.length(), m = pattern.length();
    vector<int> res, lps(m, 0);
    int len = 0, i = 1;
    while (i < m) {
        if (pattern[i] == pattern[len]) {
            len++;
            lps[i++] = len;
        } else {
            if (len != 0) len = lps[len - 1];
            else lps[i++] = 0;
        }
    }
    
    i = 0;
    int j = 0;
    while (i < n) {
        if (pattern[j] == text[i]) {
            i++; j++;
        }
        if (j == m) {
            res.push_back(i - j);
            j = lps[j - 1];
        } else if (i < n && pattern[j] != text[i]) {
            if (j != 0) j = lps[j - 1];
            else i++;
        }
    }
    return res;
}
```

## New Keywords / STL Used
- `std::vector`

## Edge Cases
- pattern length greater than text length, all identical characters.

NEXT: [[Index]]
