---
type: concept
tags: [sliding window, cpp, string]
date: 2026-06-30
---
# Longest Substring Without Repeating Characters

## Problem Statement
Given a string, find the length of the longest substring without repeating characters.

## Approach / Intuition
Use a [[Sliding Window]] alongside an array (or hash map) to keep track of the most recent index of each character. When a repeating character is encountered, advance the `left` pointer to one position after the previous occurrence of that character. Update the maximum window size at each step.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n)
- **[[Space Complexity]]:** O(1) since character set size is fixed (e.g., 256)

## Sample Code
```cpp
int lengthOfLongestSubstring(string s) {
    vector<int> lastIndex(256, -1);
    int maxLen = 0, left = 0;
    for (int right = 0; right < s.length(); ++right) {
        if (lastIndex[s[right]] != -1) {
            left = max(left, lastIndex[s[right]] + 1);
        }
        lastIndex[s[right]] = right;
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

## New Keywords / STL Used
`vector`, `std::max`

## Edge Cases
Empty string, string with all identical characters, string with all distinct characters.
