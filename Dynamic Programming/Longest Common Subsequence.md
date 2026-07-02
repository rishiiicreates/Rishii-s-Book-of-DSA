---
type: concept
tags: [dp, cpp, lcs]
date: 2026-06-30
---
# Longest Common Subsequence

## Problem Statement
Given two strings `text1` and `text2`, return the length of their longest common subsequence. If there is no common subsequence, return 0.

## Approach / Intuition
We create a 2D [[Dynamic Programming]] table where `dp[i][j]` represents the length of the longest common subsequence of `text1[0..i-1]` and `text2[0..j-1]`. If `text1[i-1] == text2[j-1]`, then `dp[i][j] = 1 + dp[i-1][j-1]`. Otherwise, `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`. Space optimization can be applied to use only two 1D arrays.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(M \times N)$ where $M$ and $N$ are the lengths of the two strings.
- **[[Space Complexity]]:** $O(\min(M, N))$ with space optimization.

## Sample Code
```cpp
#include <string>
#include <vector>
#include <algorithm>

int longestCommonSubsequence(std::string text1, std::string text2) {
    if (text1.length() < text2.length()) {
        std::swap(text1, text2);
    }
    int m = text1.length(), n = text2.length();
    std::vector<int> prev(n + 1, 0), curr(n + 1, 0);
    for (int i = 1; i <= m; ++i) {
        for (int j = 1; j <= n; ++j) {
            if (text1[i - 1] == text2[j - 1]) {
                curr[j] = 1 + prev[j - 1];
            } else {
                curr[j] = std::max(curr[j - 1], prev[j]);
            }
        }
        prev = curr;
    }
    return prev[n];
}
```

## New Keywords / STL Used
`std::max`, `std::swap`

## Edge Cases
One or both strings are empty, completely disjoint strings.
