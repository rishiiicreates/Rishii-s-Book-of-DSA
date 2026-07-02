---
type: concept
tags: [sliding window, cpp, string]
date: 2026-06-30
---
# Count Substrings with Every Vowel

## Problem Statement
Given a string of lowercase English letters, count the number of substrings that contain every vowel ('a', 'e', 'i', 'o', 'u') at least once and only consist of vowels.

## Approach / Intuition
This requires finding substrings comprised entirely of vowels. We can find contiguous segments of vowels. Within those segments, use a [[Sliding Window]] combined with a frequency map to count valid substrings. Expand the `right` pointer, and when all 5 vowels are present, count all extensions to the right, then shrink from the `left`.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n)
- **[[Space Complexity]]:** O(1) since there are only 5 vowels

## Sample Code
```cpp
bool isVowel(char c) {
    return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
}

int countVowelSubstrings(string word) {
    int res = 0;
    for (int i = 0; i < word.length(); ++i) {
        unordered_set<char> vowels;
        for (int j = i; j < word.length(); ++j) {
            if (!isVowel(word[j])) break;
            vowels.insert(word[j]);
            if (vowels.size() == 5) res++;
        }
    }
    return res;
}
```

## New Keywords / STL Used
`unordered_set`

## Edge Cases
String with no vowels, string with less than 5 distinct vowels, empty string.
