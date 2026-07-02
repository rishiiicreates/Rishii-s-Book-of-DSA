---
type: concept
tags: [cpp, string_matching, palindromes]
date: 2026-06-30
---
# Manacher's Algorithm

## Problem Statement
Find the longest palindromic substring in a given string in linear time.

## Approach / Intuition
[[Manacher's Algorithm]] expands palindromes around centers but reuses already computed palindrome lengths using properties of symmetry. We insert dummy characters (e.g., `#`) between original characters to handle both even and odd length palindromes uniformly.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

string longestPalindrome(string s) {
    if (s.empty()) return "";
    string t = "^";
    for (char c : s) {
        t += "#" + string(1, c);
    }
    t += "#$";
    int n = t.length();
    vector<int> p(n, 0);
    int c = 0, r = 0, maxLen = 0, centerIndex = 0;
    
    for (int i = 1; i < n - 1; i++) {
        int i_mirror = 2 * c - i;
        if (r > i) {
            p[i] = min(r - i, p[i_mirror]);
        }
        while (t[i + 1 + p[i]] == t[i - 1 - p[i]]) {
            p[i]++;
        }
        if (i + p[i] > r) {
            c = i;
            r = i + p[i];
        }
        if (p[i] > maxLen) {
            maxLen = p[i];
            centerIndex = i;
        }
    }
    
    int start = (centerIndex - maxLen - 1) / 2;
    return s.substr(start, maxLen);
}
```

## New Keywords / STL Used
`std::min`, `std::string::substr`

## Edge Cases
Empty string, single character string, string with all same characters.
