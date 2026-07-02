---
type: concept
tags: [two-pointer, cpp, string]
date: 2026-06-30
---
# Valid Palindrome

## Problem Statement
Determine if a given string is a palindrome, considering only alphanumeric characters and ignoring cases.

## Approach / Intuition
Set one pointer at the start of the string and another at the end. Move them towards the center, skipping non-alphanumeric characters. Compare the characters at both pointers (converted to lowercase). If they ever mismatch, it's not a palindrome. This is an application of converging [[Two Pointers]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(n)
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
bool isPalindrome(string s) {
    int left = 0;
    int right = s.length() - 1;
    while (left < right) {
        while (left < right && !isalnum(s[left])) {
            left++;
        }
        while (left < right && !isalnum(s[right])) {
            right--;
        }
        if (tolower(s[left]) != tolower(s[right])) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

## New Keywords / STL Used
`isalnum`, `tolower`

## Edge Cases
Empty string, string with only spaces or punctuation, single character.
