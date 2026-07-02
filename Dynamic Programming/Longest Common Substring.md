---
type: concept
tags: [dp, cpp, string]
date: 2026-06-30
---
# Longest Common Substring

## Problem Statement
Given two strings, find the length of the longest common substring between them. A substring is a contiguous sequence of characters within a string.

## Approach / Intuition
Use a 2D [[Dynamic Programming]] table where `dp[i][j]` stores the length of the longest common suffix of the substrings ending at `s1[i-1]` and `s2[j-1]`. If the characters match, `dp[i][j] = dp[i-1][j-1] + 1`; otherwise, it's 0. The max value in the table is the answer.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n * m)
- **[[Space Complexity]]:** O(n * m) (can be optimized to O(m))

## Sample Code
```cpp
int longestCommonSubstr(string S1, string S2, int n, int m) {
    vector<int> prev(m + 1, 0), curr(m + 1, 0);
    int max_len = 0;
    
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            if (S1[i - 1] == S2[j - 1]) {
                curr[j] = prev[j - 1] + 1;
                max_len = max(max_len, curr[j]);
            } else {
                curr[j] = 0;
            }
        }
        prev = curr;
    }
    return max_len;
}
```

## New Keywords / STL Used
`vector`, `max`, `string`

## Edge Cases
Empty strings, completely different strings, identical strings.
