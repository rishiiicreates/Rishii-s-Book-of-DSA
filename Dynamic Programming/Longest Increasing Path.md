# Longest Increasing Path

## Problem Statement
- given an `m x n` integers matrix, return the length of the longest increasing path. You can move up, down, left, or right, but not diagonally or outside the boundary.

## Approach / Intuition
- this combines [[DFS]] with [[Memoization]]. Start a DFS from each cell and compute the longest increasing path from it. If a cell's longest path is already computed in our cache, reuse it. The global max is the longest among all DFS starts.

## Time & Space Complexity
- **[[time Complexity]]:** O(m * n)
- **[[space Complexity]]:** O(m * n)

## Sample Code
```cpp
int dfs(int r, int c, vector<vector<int>>& matrix, vector<vector<int>>& cache, int m, int n) {
    if (cache[r][c] != 0) return cache[r][c];
    
    int dr[] = {-1, 1, 0, 0};
    int dc[] = {0, 0, -1, 1};
    int max_len = 1;
    
    for (int i = 0; i < 4; ++i) {
        int nr = r + dr[i];
        int nc = c + dc[i];
        if (nr >= 0 && nr < m && nc >= 0 && nc < n && matrix[nr][nc] > matrix[r][c]) {
            max_len = max(max_len, 1 + dfs(nr, nc, matrix, cache, m, n));
        }
    }
    return cache[r][c] = max_len;
}

int longestIncreasingPath(vector<vector<int>>& matrix) {
    if (matrix.empty()) return 0;
    int m = matrix.size(), n = matrix[0].size();
    vector<vector<int>> cache(m, vector<int>(n, 0));
    int longest = 0;
    
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            longest = max(longest, dfs(i, j, matrix, cache, m, n));
        }
    }
    return longest;
}
```

## New Keywords / STL Used
- `vector`, `max`

## Edge Cases
- empty [[Matrix]], all elements equal (no strictly increasing path), 1x1 matrix.

NEXT: [[Index]]
