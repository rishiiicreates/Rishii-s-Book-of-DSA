---
type: concept
tags: [dp, cpp, interval]
date: 2026-06-30
---
# Minimum Cost to Cut a Stick

## Problem Statement
Given a wooden stick of length `n` and an array of cut positions, find the minimum total cost to make all cuts. Cost of a cut is the length of the stick being cut.

## Approach / Intuition
We sort the cuts and add the stick endpoints `0` and `n`. This becomes an interval [[Dynamic Programming]] problem. We define `dp[i][j]` as the minimum cost to perform all cuts between index `i` and `j` in the `cuts` array. For any cut `k` between `i` and `j`, the cost is `cuts[j] - cuts[i] + dp[i][k] + dp[k][j]`.

## Time & Space Complexity
- **[[Time Complexity]]:** O(M^3), where M is number of cuts
- **[[Space Complexity]]:** O(M^2)

## Sample Code
```cpp
#include <vector>
#include <algorithm>
#include <climits>

using namespace std;

int minCost(int n, vector<int>& cuts) {
    cuts.push_back(0);
    cuts.push_back(n);
    sort(cuts.begin(), cuts.end());
    
    int m = cuts.size();
    vector<vector<int>> dp(m, vector<int>(m, 0));
    
    for (int len = 2; len < m; ++len) {
        for (int i = 0; i < m - len; ++i) {
            int j = i + len;
            dp[i][j] = INT_MAX;
            for (int k = i + 1; k < j; ++k) {
                dp[i][j] = min(dp[i][j], cuts[j] - cuts[i] + dp[i][k] + dp[k][j]);
            }
        }
    }
    
    return dp[0][m - 1];
}
```

## New Keywords / STL Used
`std::sort`, `INT_MAX`

## Edge Cases
No cuts to make, cuts already sorted, overlapping cuts (invalid per constraint but theoretically possible).
