---
type: concept
tags: [sliding window, cpp, hash-map]
date: 2026-06-30
---
# Minimum Window Substring

## Problem Statement
Given two strings `s` and `t`, return the minimum window substring of `s` such that every character in `t` (including duplicates) is included in the window.

## Approach / Intuition
Use a [[Sliding Window]] and an array to track character frequencies. Expand the window to the right until it contains all characters of `t`. Then, shrink the window from the left to find the minimum length while keeping the required characters.

## Time & Space Complexity
- **[[Time Complexity]]:** O(S + T)
- **[[Space Complexity]]:** O(1) since character sets are bounded (e.g., ASCII).

## Sample Code
```cpp
string minWindow(string s, string t) {
    vector<int> count(128, 0);
    for (char c : t) count[c]++;
    int left = 0, right = 0, required = t.size();
    int minLen = INT_MAX, start = 0;
    
    while (right < s.size()) {
        if (count[s[right]] > 0) required--;
        count[s[right]]--;
        right++;
        
        while (required == 0) {
            if (right - left < minLen) {
                minLen = right - left;
                start = left;
            }
            count[s[left]]++;
            if (count[s[left]] > 0) required++;
            left++;
        }
    }
    return minLen == INT_MAX ? "" : s.substr(start, minLen);
}
```

## New Keywords / STL Used
- `std::vector` for frequency counting

## Edge Cases
- `s` is shorter than `t`
- Window covers the entire string `s`
- No such window exists
