---
type: concept
tags: [dp, cpp, matrix]
date: 2026-06-30
---
# Matrix Chain Multiplication

## Problem Statement
Given a sequence of matrices, find the most efficient way to multiply these matrices together by minimizing the number of scalar multiplications.

## Approach / Intuition
This is a classic interval [[Dynamic Programming]] problem. We evaluate all possible splits to multiply the matrices. `dp[i][j]` represents the minimum cost to multiply matrices from index `i` to `j`. We iterate over all possible chain lengths, and for each interval `[i, j]`, we try partitioning at every possible `k`. Cost is `dp[i][k] + dp[k+1][j] + cost_to_multiply_results`.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N^3)
- **[[Space Complexity]]:** O(N^2)

## Sample Code
```cpp
#include <vector>
#include <climits>
#include <algorithm>

using namespace std;

int matrixMultiplication(int N, vector<int>& arr) {
    vector<vector<int>> dp(N, vector<int>(N, 0));
    
    for (int len = 2; len < N; ++len) {
        for (int i = 1; i < N - len + 1; ++i) {
            int j = i + len - 1;
            dp[i][j] = INT_MAX;
            for (int k = i; k < j; ++k) {
                int cost = dp[i][k] + dp[k+1][j] + arr[i-1] * arr[k] * arr[j];
                dp[i][j] = min(dp[i][j], cost);
            }
        }
    }
    return dp[1][N-1];
}
```

## New Keywords / STL Used
`INT_MAX`, `std::min`

## Edge Cases
Only two matrices to multiply, dimensions resulting in very large costs, small N.
