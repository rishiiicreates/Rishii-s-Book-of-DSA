# Interleaving String

## Problem Statement
- given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an interleaving of `s1` and `s2`.

## Approach / Intuition
- use a 2D [[Dynamic Programming]] boolean table where `dp[i][j]` means `s1[0..i-1]` and `s2[0..j-1]` interleave to form `s3[0..i+j-1]`. Check character matches sequentially. If length doesn't match, return false immediately. Space can be optimized to 1D.

## Time & Space Complexity
- **[[time Complexity]]:** O(n * m)
- **[[space Complexity]]:** O(m)

## Sample Code
```cpp
bool isInterleave(string s1, string s2, string s3) {
    if (s1.length() + s2.length() != s3.length()) return false;
    
    int n = s1.length(), m = s2.length();
    vector<bool> dp(m + 1, false);
    
    for (int i = 0; i <= n; ++i) {
        for (int j = 0; j <= m; ++j) {
            if (i == 0 && j == 0) {
                dp[j] = true;
            } else if (i == 0) {
                dp[j] = dp[j - 1] && s2[j - 1] == s3[i + j - 1];
            } else if (j == 0) {
                dp[j] = dp[j] && s1[i - 1] == s3[i + j - 1];
            } else {
                dp[j] = (dp[j] && s1[i - 1] == s3[i + j - 1]) || 
                        (dp[j - 1] && s2[j - 1] == s3[i + j - 1]);
            }
        }
    }
    return dp[m];
}
```

## New Keywords / STL Used
- `vector`, `string`, `length()`

## Edge Cases
- empty strings for `s1` and `s2`, character matching but order swapped, string lengths sum mismatch.

NEXT: [[Index]]
