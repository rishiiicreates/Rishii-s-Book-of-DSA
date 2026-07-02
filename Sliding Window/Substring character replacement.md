---
type: concept
tags: [sliding window, cpp, string]
date: 2026-06-30
---
# Longest Repeating Character Replacement

## Problem Statement
Given a string `s` and an integer `k`, you can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times. Find the length of the longest substring containing the same letter you can get.

## Approach / Intuition
Utilize a [[Sliding Window]] while keeping a frequency map of characters in the current window. The length of the window minus the frequency of the most common character gives the number of replacements needed. If this exceeds `k`, shrink the window from the left.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n)
- **[[Space Complexity]]:** O(1) size 26 array

## Sample Code
```cpp
int characterReplacement(string s, int k) {
    vector<int> count(26, 0);
    int maxCount = 0, maxLen = 0, left = 0;
    for (int right = 0; right < s.length(); ++right) {
        count[s[right] - 'A']++;
        maxCount = max(maxCount, count[s[right] - 'A']);
        if (right - left + 1 - maxCount > k) {
            count[s[left] - 'A']--;
            left++;
        }
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
`k = 0`, string is entirely one character, `k` is larger than the string length.
