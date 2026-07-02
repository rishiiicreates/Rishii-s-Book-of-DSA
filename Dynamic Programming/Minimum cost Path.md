---
type: concept
tags: [dp, cpp, grid_dp]
date: 2026-06-30
---
# Minimum cost Path

## Problem Statement
Given an $m \times n$ grid filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path. You can only move either down or right at any point in time.

## Approach / Intuition
To minimize the path sum to reach cell $(i, j)$, we take the current cell's value and add it to the minimum of the path sums to reach the cell directly above $(i-1, j)$ and the cell to the left $(i, j-1)$. We use [[Dynamic Programming]] and can optimize space to just a 1D array representing the previous row.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(M \times N)$
- **[[Space Complexity]]:** $O(N)$ using 1D space optimization.

## Sample Code
```cpp
#include <vector>
#include <algorithm>

int minPathSum(std::vector<std::vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    std::vector<int> dp(n, 0);
    dp[0] = grid[0][0];
    for (int j = 1; j < n; ++j) {
        dp[j] = dp[j - 1] + grid[0][j];
    }
    for (int i = 1; i < m; ++i) {
        dp[0] += grid[i][0];
        for (int j = 1; j < n; ++j) {
            dp[j] = std::min(dp[j], dp[j - 1]) + grid[i][j];
        }
    }
    return dp[n - 1];
}
```

## New Keywords / STL Used
`std::min`, `std::vector`

## Edge Cases
$1 \times 1$ grid.
