# Unique Paths in a Grid

## Problem Statement
- a robot is located at the top-left corner of a `m x n` grid. It can only move either down or right at any point in time. Find the number of possible unique paths to the bottom-right corner.

## Approach / Intuition
- the number of ways to reach cell `(i, j)` is the sum of ways to reach `(i-1, j)` (from above) and `(i, j-1)` (from left). We use a 2D [[Dynamic Programming]] table or just a 1D array to optimize space since we only need the previous row's results.

## Time & Space Complexity
- **[[time Complexity]]:** O(m * n)
- **[[space Complexity]]:** O(n)

## Sample Code
```cpp
int uniquePaths(int m, int n) {
    vector<int> dp(n, 1);
    for (int i = 1; i < m; ++i) {
        for (int j = 1; j < n; ++j) {
            dp[j] += dp[j - 1];
        }
    }
    return dp[n - 1];
}
```

## New Keywords / STL Used
- `vector`

## Edge Cases
- grid size `1 x n` or `m x 1` (only 1 path), grid is `1 x 1`.

NEXT: [[Index]]
