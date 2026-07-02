---
type: concept
tags: [dp, cpp, string]
date: 2026-06-30
---
# Longest Palindromic Substring

## Problem Statement
Given a string `s`, return the longest palindromic substring in `s`.

## Approach / Intuition
We can solve this by expanding around the center for each character or by using [[Dynamic Programming]]. In DP, a substring `s[i..j]` is a palindrome if `s[i] == s[j]` and `s[i+1..j-1]` is a palindrome. Expand around center is often faster in practice.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n^2)
- **[[Space Complexity]]:** O(1) for expand around center (DP is O(n^2))

## Sample Code
```cpp
string longestPalindrome(string s) {
    if (s.empty()) return "";
    int start = 0, maxLen = 1, n = s.length();
    
    auto expandAroundCenter = [&](int left, int right) {
        while (left >= 0 && right < n && s[left] == s[right]) {
            left--;
            right++;
        }
        return right - left - 1;
    };
    
    for (int i = 0; i < n; i++) {
        int len1 = expandAroundCenter(i, i);
        int len2 = expandAroundCenter(i, i + 1);
        int len = max(len1, len2);
        if (len > maxLen) {
            maxLen = len;
            start = i - (len - 1) / 2;
        }
    }
    return s.substr(start, maxLen);
}
```

## New Keywords / STL Used
`string`, `substr`, `max`, lambda function

## Edge Cases
Single character string, all identical characters, empty string.
