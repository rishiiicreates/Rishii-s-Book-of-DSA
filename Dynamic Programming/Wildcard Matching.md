# Wildcard Matching

## Problem Statement
- given an input string `s` and a pattern `p`, implement wildcard pattern matching with support for `?` (matches any single character) and `*` (matches any sequence of characters, including the empty sequence).

## Approach / Intuition
- use a 2D [[Dynamic Programming]] array where `dp[i][j]` is true if `s[0..i-1]` matches `p[0..j-1]`. If `p` has `?` or characters match, take diagonal value. If `p` has `*`, it can match zero characters (`dp[i][j-1]`) or match current character (`dp[i-1][j]`).

## Time & Space Complexity
- **[[time Complexity]]:** O(n * m)
- **[[space Complexity]]:** O(n * m) (can be optimized to O(m))

## Sample Code
```cpp
bool isMatch(string s, string p) {
    int n = s.size(), m = p.size();
    vector<vector<bool>> dp(n + 1, vector<bool>(m + 1, false));
    dp[0][0] = true;
    
    for (int j = 1; j <= m; ++j) {
        if (p[j - 1] == '*') dp[0][j] = dp[0][j - 1];
    }
    
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            if (p[j - 1] == s[i - 1] || p[j - 1] == '?') {
                dp[i][j] = dp[i - 1][j - 1];
            } else if (p[j - 1] == '*') {
                dp[i][j] = dp[i - 1][j] || dp[i][j - 1];
            }
        }
    }
    return dp[n][m];
}
```

## New Keywords / STL Used
- `vector`, `string`

## Edge Cases
- empty string, pattern with only `*`, pattern with no wildcards.

NEXT: [[Index]]
