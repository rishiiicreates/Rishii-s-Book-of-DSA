---
type: concept
tags: [dp, cpp, string]
date: 2026-06-30
---
# Shortest Common Supersequence

## Problem Statement
Given two strings `str1` and `str2`, return the shortest string that has both `str1` and `str2` as subsequences. If there are multiple valid strings, return any of them.

## Approach / Intuition
First find the [[Longest Common Subsequence]] (LCS) using [[Dynamic Programming]]. Then backtrack through the DP table from the bottom right to construct the supersequence, picking matching characters once and non-matching characters from both strings as needed.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n * m)
- **[[Space Complexity]]:** O(n * m)

## Sample Code
```cpp
string shortestCommonSupersequence(string str1, string str2) {
    int n = str1.size(), m = str2.size();
    vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
    
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            if (str1[i - 1] == str2[j - 1]) dp[i][j] = 1 + dp[i - 1][j - 1];
            else dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
        }
    }
    
    string res = "";
    int i = n, j = m;
    while (i > 0 && j > 0) {
        if (str1[i - 1] == str2[j - 1]) {
            res += str1[i - 1];
            --i; --j;
        } else if (dp[i - 1][j] > dp[i][j - 1]) {
            res += str1[i - 1];
            --i;
        } else {
            res += str2[j - 1];
            --j;
        }
    }
    
    while (i > 0) res += str1[--i];
    while (j > 0) res += str2[--j];
    
    reverse(res.begin(), res.end());
    return res;
}
```

## New Keywords / STL Used
`vector`, `string`, `reverse`

## Edge Cases
One string is empty, strings have no common characters, strings are identical.
