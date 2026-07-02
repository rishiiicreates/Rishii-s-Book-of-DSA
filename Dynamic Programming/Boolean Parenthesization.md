---
type: concept
tags: [dp, cpp, interval]
date: 2026-06-30
---
# Boolean Parenthesization

## Problem Statement
Given a boolean expression with symbols `T`, `F`, and operators `&`, `|`, `^`, find the number of ways to parenthesize the expression so it evaluates to `True`.

## Approach / Intuition
We use interval [[Dynamic Programming]]. We need two 3D arrays or state caching: `dp[i][j][isTrue]` representing the number of ways to evaluate the substring from `i` to `j` to boolean value `isTrue`. We split at every operator `k` between `i` and `j`, recursively evaluate the left and right subexpressions, and combine their counts based on the operator.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N^3)
- **[[Space Complexity]]:** O(N^2)

## Sample Code
```cpp
#include <string>
#include <vector>

using namespace std;

int countWays(int n, string s) {
    int MOD = 1003;
    vector<vector<vector<int>>> dp(n, vector<vector<int>>(n, vector<int>(2, 0)));
    
    for (int i = 0; i < n; i += 2) {
        if (s[i] == 'T') dp[i][i][1] = 1;
        else dp[i][i][0] = 1;
    }
    
    for (int len = 3; len <= n; len += 2) {
        for (int i = 0; i <= n - len; i += 2) {
            int j = i + len - 1;
            for (int k = i + 1; k < j; k += 2) {
                int leftT = dp[i][k - 1][1], leftF = dp[i][k - 1][0];
                int rightT = dp[k + 1][j][1], rightF = dp[k + 1][j][0];
                
                if (s[k] == '&') {
                    dp[i][j][1] = (dp[i][j][1] + (leftT * rightT) % MOD) % MOD;
                    dp[i][j][0] = (dp[i][j][0] + (leftT * rightF + leftF * rightT + leftF * rightF) % MOD) % MOD;
                } else if (s[k] == '|') {
                    dp[i][j][1] = (dp[i][j][1] + (leftT * rightF + leftF * rightT + leftT * rightT) % MOD) % MOD;
                    dp[i][j][0] = (dp[i][j][0] + (leftF * rightF) % MOD) % MOD;
                } else if (s[k] == '^') {
                    dp[i][j][1] = (dp[i][j][1] + (leftT * rightF + leftF * rightT) % MOD) % MOD;
                    dp[i][j][0] = (dp[i][j][0] + (leftT * rightT + leftF * rightF) % MOD) % MOD;
                }
            }
        }
    }
    
    return dp[0][n - 1][1];
}
```

## New Keywords / STL Used
None specifically beyond basic vectors.

## Edge Cases
Single boolean value, expression missing operators, purely true/false sequence.
