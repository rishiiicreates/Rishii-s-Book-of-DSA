# Edit Distance

## Problem Statement
- given two strings `word1` and `word2`, return the minimum number of operations required to convert `word1` to `word2`. The allowed operations are insert, delete, or replace a character.

## Approach / Intuition
- using [[Dynamic Programming]], let `dp[i][j]` be the edit distance between `word1[0..i-1]` and `word2[0..j-1]`. If the characters match, `dp[i][j] = dp[i-1][j-1]`. If they differ, we take the minimum of inserting (`dp[i][j-1]`), deleting (`dp[i-1][j]`), and replacing (`dp[i-1][j-1]`) and add 1. We can optimize space to $O(N)$.

## Time & Space Complexity
- **[[time Complexity]]:** $O(M \times N)$ where $M$ and $N$ are string lengths.
- **[[space Complexity]]:** $O(\min(M, N))$ with space optimization.

## Sample Code
```cpp
#include <string>
#include <vector>
#include <algorithm>

int minDistance(std::string word1, std::string word2) {
    int m = word1.length(), n = word2.length();
    std::vector<int> prev(n + 1, 0), curr(n + 1, 0);
    for (int j = 0; j <= n; ++j) prev[j] = j;
    for (int i = 1; i <= m; ++i) {
        curr[0] = i;
        for (int j = 1; j <= n; ++j) {
            if (word1[i - 1] == word2[j - 1]) {
                curr[j] = prev[j - 1];
            } else {
                curr[j] = 1 + std::min({prev[j - 1], prev[j], curr[j - 1]});
            }
        }
        prev = curr;
    }
    return prev[n];
}
```

## New Keywords / STL Used
- `std::min` with initializer list.

## Edge Cases
- one string is empty, both strings are identical.

NEXT: [[Index]]
