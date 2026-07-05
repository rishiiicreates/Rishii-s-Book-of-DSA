# Palindrome Partitioning II

## Problem Statement
- given a string, find the minimum number of cuts needed to partition it such that every substring is a palindrome.

## Approach / Intuition
- first, compute a boolean 2D array `isPal[i][j]` to quickly check if substring `s[i..j]` is a palindrome. Then, use a 1D [[Dynamic Programming]] array `dp[i]` which represents the minimum cuts for substring `s[0..i]`. If `s[0..i]` is a palindrome, no cuts are needed. Otherwise, we try placing a cut at `j` such that `s[j+1..i]` is a palindrome, minimizing `dp[j] + 1`.

## Time & Space Complexity
- **[[time Complexity]]:** O(N^2)
- **[[space Complexity]]:** O(N^2)

## Sample Code
```cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int minCut(string s) {
    int n = s.length();
    vector<vector<bool>> isPal(n, vector<bool>(n, false));
    
    for (int i = n - 1; i >= 0; --i) {
        for (int j = i; j < n; ++j) {
            if (s[i] == s[j] && (j - i <= 2 || isPal[i + 1][j - 1])) {
                isPal[i][j] = true;
            }
        }
    }
    
    vector<int> dp(n, 0);
    for (int i = 0; i < n; ++i) {
        if (isPal[0][i]) {
            dp[i] = 0;
            continue;
        }
        dp[i] = i;
        for (int j = 0; j < i; ++j) {
            if (isPal[j + 1][i]) {
                dp[i] = min(dp[i], dp[j] + 1);
            }
        }
    }
    
    return dp[n - 1];
}
```

## New Keywords / STL Used
- `std::min`

## Edge Cases
- string is already a single palindrome, string with all unique characters, string of length 1.

NEXT: [[Index]]
