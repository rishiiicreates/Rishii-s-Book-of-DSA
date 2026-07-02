---
type: concept
tags: [dsa, cpp, string-matching, palindrome]
date: 2026-06-30
---
# Longest Palindromic Substring (String Matching Version)

## Problem Statement
Find the longest substring of a given string that is also a palindrome.

## Approach / Intuition
While typically solved with DP or expanding around centers, from a [[String Matching]] perspective, a palindrome reads the same forwards and backwards. We can look for the longest common substring between the string and its reverse. However, we must verify that the matched substring in the reversed string actually corresponds to the same indices in the original string. Alternatively, expand around centers directly in O(N^2) or use [[Manacher's Algorithm]] for O(N).

## Time & Space Complexity
- **[[Time Complexity]]:** O(n^2) for expand around center
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
string longestPalindrome(string s) {
    if (s.empty()) return "";
    int start = 0, maxLen = 1;
    for (int i = 0; i < s.length(); ++i) {
        int l = i, r = i;
        while (l >= 0 && r < s.length() && s[l] == s[r]) {
            if (r - l + 1 > maxLen) {
                maxLen = r - l + 1;
                start = l;
            }
            l--; r++;
        }
        l = i, r = i + 1;
        while (l >= 0 && r < s.length() && s[l] == s[r]) {
            if (r - l + 1 > maxLen) {
                maxLen = r - l + 1;
                start = l;
            }
            l--; r++;
        }
    }
    return s.substr(start, maxLen);
}
```

## New Keywords / STL Used
`std::string::substr`

## Edge Cases
String is empty, all identical characters, string is already a palindrome.
