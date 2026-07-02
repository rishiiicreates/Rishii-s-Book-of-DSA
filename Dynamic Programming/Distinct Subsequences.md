---
type: concept
tags: [dp, cpp, string_dp]
date: 2026-06-30
---
# Distinct Subsequences

## Problem Statement
Given two strings $s$ and $t$, return the number of distinct subsequences of $s$ which equals $t$.

## Approach / Intuition
Let `dp[i][j]` be the number of distinct subsequences of `s[0..i-1]` that equal `t[0..j-1]`. If `s[i-1] == t[j-1]`, we can either use this match (`dp[i-1][j-1]`) or not (`dp[i-1][j]`). So, `dp[i][j] = dp[i-1][j-1] + dp[i-1][j]`. If they don't match, we cannot use it, so `dp[i][j] = dp[i-1][j]`. We can apply [[Dynamic Programming]] and space optimization using a 1D array, iterating backwards for the inner loop.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(M \times N)$
- **[[Space Complexity]]:** $O(N)$ using space optimization, where $N$ is length of $t$.

## Sample Code
```cpp
#include <string>
#include <vector>

int numDistinct(std::string s, std::string t) {
    int m = s.length(), n = t.length();
    std::vector<double> dp(n + 1, 0);
    dp[0] = 1;
    for (int i = 1; i <= m; ++i) {
        for (int j = n; j >= 1; --j) {
            if (s[i - 1] == t[j - 1]) {
                dp[j] += dp[j - 1];
            }
        }
    }
    return (int)dp[n];
}
```

## New Keywords / STL Used
`double` used in DP array to avoid overflow before truncation to `int` in some large test cases on LeetCode.

## Edge Cases
$s$ is shorter than $t$ (returns 0), $t$ is empty (returns 1).
