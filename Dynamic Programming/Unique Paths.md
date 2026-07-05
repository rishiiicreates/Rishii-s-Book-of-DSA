# Unique Paths

## Problem Statement
- a robot is located at the top-left corner of an $m \times n$ grid. The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid. How many possible unique paths are there?

## Approach / Intuition
- the number of unique paths to reach cell $(i, j)$ is the sum of unique paths to reach the cell above $(i-1, j)$ and the cell to the left $(i, j-1)$. This gives the [[Dynamic Programming]] transition. We can optimize space by only maintaining the values of the previous row.

## Time & Space Complexity
- **[[time Complexity]]:** $O(M \times N)$ to fill the DP table.
- **[[space Complexity]]:** $O(N)$ using space optimization with a 1D array.

## Sample Code
```cpp
#include <vector>

int uniquePaths(int m, int n) {
    std::vector<int> dp(n, 1);
    for (int i = 1; i < m; ++i) {
        for (int j = 1; j < n; ++j) {
            dp[j] += dp[j - 1];
        }
    }
    return dp[n - 1];
}
```

## New Keywords / STL Used
- `std::vector`

## Edge Cases
- $m = 1$ or $n = 1$, where the answer is always 1.

NEXT: [[Index]]
