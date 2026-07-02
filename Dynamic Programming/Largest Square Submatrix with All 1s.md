---
type: concept
tags: [dp, cpp, grid_dp]
date: 2026-06-30
---
# Largest Square Submatrix with All 1s

## Problem Statement
Given an $m \times n$ binary matrix filled with 0s and 1s, find the largest square containing only 1s and return its area.

## Approach / Intuition
Let `dp[i][j]` represent the side length of the maximum square whose bottom-right corner is at cell $(i, j)$ in the matrix. If the cell is '1', the size of the square is bounded by its top, left, and top-left neighbors. Thus, `dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1`. This [[Dynamic Programming]] approach can be space-optimized to use only a 1D array.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(M \times N)$
- **[[Space Complexity]]:** $O(N)$ with space optimization.

## Sample Code
```cpp
#include <vector>
#include <algorithm>

int maximalSquare(std::vector<std::vector<char>>& matrix) {
    if (matrix.empty() || matrix[0].empty()) return 0;
    int m = matrix.size(), n = matrix[0].size();
    std::vector<int> dp(n + 1, 0);
    int maxSq = 0, prev = 0;
    for (int i = 1; i <= m; ++i) {
        for (int j = 1; j <= n; ++j) {
            int temp = dp[j];
            if (matrix[i - 1][j - 1] == '1') {
                dp[j] = std::min({dp[j - 1], dp[j], prev}) + 1;
                maxSq = std::max(maxSq, dp[j]);
            } else {
                dp[j] = 0;
            }
            prev = temp;
        }
    }
    return maxSq * maxSq;
}
```

## New Keywords / STL Used
`std::min` with initializer list, `std::max`

## Edge Cases
Matrix with all 0s, matrix with a single 1.
