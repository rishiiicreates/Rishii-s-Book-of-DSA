# Word Break

## Problem Statement
- given a string $s$ and a dictionary of strings `wordDict`, return true if $s$ can be segmented into a space-separated sequence of one or more dictionary words.

## Approach / Intuition
- we use a boolean DP array where `dp[i]` indicates whether the prefix of $s$ of length $i$ can be broken into valid words. For each length $i$, we check if there exists a $j < i$ such that `dp[j]` is true and the substring from $j$ to $i$ is in `wordDict`. Using an `unordered_set` for the dictionary ensures $O(1)$ lookups.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N^3)$ where $N$ is string length (due to substring operation), or $O(N^2)$ with [[Trie]].
- **[[space Complexity]]:** $O(N)$ for the DP array and the dictionary storage.

## Sample Code
```cpp
#include <string>
#include <vector>
#include <unordered_set>

bool wordBreak(std::string s, std::vector<std::string>& wordDict) {
    std::unordered_set<std::string> dict(wordDict.begin(), wordDict.end());
    std::vector<bool> dp(s.length() + 1, false);
    dp[0] = true;
    for (int i = 1; i <= s.length(); ++i) {
        for (int j = 0; j < i; ++j) {
            if (dp[j] && dict.count(s.substr(j, i - j))) {
                dp[i] = true;
                break;
            }
        }
    }
    return dp[s.length()];
}
```

## New Keywords / STL Used
- `std::unordered_set`, `substr`

## Edge Cases
- empty dictionary, string cannot be broken, very long strings.

NEXT: [[Index]]
