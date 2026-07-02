---
type: concept
tags: [sliding window, cpp, string-manipulation]
date: 2026-06-30
---
# Equal substring with cost less than K

## Problem Statement
Given two strings `s` and `t`, find the maximum length of a substring of `s` that can be changed to be the same as the corresponding substring of `t` with a total cost less than or equal to `maxCost`. The cost of changing a character is the absolute difference in their ASCII values.

## Approach / Intuition
Apply a [[Sliding Window]] approach. Iterate with a right pointer, adding the absolute difference of characters to a running cost. If the cost exceeds `maxCost`, advance the left pointer to reduce the cost.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the length of the strings.
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
int equalSubstring(string s, string t, int maxCost) {
    int left = 0, cost = 0, maxLen = 0;
    for (int right = 0; right < s.length(); ++right) {
        cost += abs(s[right] - t[right]);
        while (cost > maxCost) {
            cost -= abs(s[left] - t[left]);
            left++;
        }
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

## New Keywords / STL Used
- `std::abs`, `std::max`

## Edge Cases
- `maxCost` is 0
- Strings are already identical
- Impossible to match even one character within `maxCost`
