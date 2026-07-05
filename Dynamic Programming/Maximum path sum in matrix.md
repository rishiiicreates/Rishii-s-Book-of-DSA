# Maximum path sum in matrix

## Problem Statement
- given an `N x M` matrix of integers, find the maximum path sum from any cell in the first row to any cell in the last row. You can move down, diagonally left, or diagonally right.

## Approach / Intuition
- use [[Dynamic Programming]] to iterate through each row. For each cell, add the maximum value among the three reachable cells directly above it. The answer will be the maximum value in the final row. Space can be optimized by keeping only the previous row.

## Time & Space Complexity
- **[[time Complexity]]:** O(N * M)
- **[[space Complexity]]:** O(M)

## Sample Code
```cpp
int maximumPath(int N, int M, vector<vector<int>> Matrix) {
    vector<int> prev = Matrix[0];
    vector<int> curr(M, 0);
    
    for (int i = 1; i < N; ++i) {
        for (int j = 0; j < M; ++j) {
            int up = prev[j];
            int left = (j > 0) ? prev[j - 1] : -1e9;
            int right = (j < M - 1) ? prev[j + 1] : -1e9;
            curr[j] = Matrix[i][j] + max({up, left, right});
        }
        prev = curr;
    }
    
    int max_sum = -1e9;
    for (int j = 0; j < M; ++j) {
        max_sum = max(max_sum, prev[j]);
    }
    return max_sum;
}
```

## New Keywords / STL Used
- `max` with initializer list, `vector`

## Edge Cases
- single column [[Matrix]], single row matrix, all negative numbers.

NEXT: [[Index]]
