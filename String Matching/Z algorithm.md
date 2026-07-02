---
type: concept
tags: [cpp, string_matching]
date: 2026-06-30
---
# Z algorithm

## Problem Statement
Find all occurrences of a pattern in a text using linear time complexity.

## Approach / Intuition
The [[Z Algorithm]] computes a Z-array for a combined string `pattern + "$" + text`. The value `Z[i]` represents the length of the longest substring starting from `i` that is also a prefix of the string.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N + M)$
- **[[Space Complexity]]:** $O(N + M)$

## Sample Code
```cpp
#include <string>
#include <vector>

using namespace std;

vector<int> zAlgorithm(string text, string pattern) {
    string concat = pattern + "$" + text;
    int l = concat.length();
    vector<int> z(l, 0);
    int L = 0, R = 0;
    for (int i = 1; i < l; i++) {
        if (i > R) {
            L = R = i;
            while (R < l && concat[R - L] == concat[R]) R++;
            z[i] = R - L;
            R--;
        } else {
            int k = i - L;
            if (z[k] < R - i + 1) {
                z[i] = z[k];
            } else {
                L = i;
                while (R < l && concat[R - L] == concat[R]) R++;
                z[i] = R - L;
                R--;
            }
        }
    }
    vector<int> res;
    for (int i = 0; i < l; i++) {
        if (z[i] == pattern.length()) res.push_back(i - pattern.length() - 1);
    }
    return res;
}
```

## New Keywords / STL Used
String concatenation

## Edge Cases
Character `$` appearing in text or pattern.
