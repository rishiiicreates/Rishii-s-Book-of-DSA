---
type: concept
tags: [dp, cpp, knapsack]
date: 2026-06-30
---
# 0/1 Knapsack

## Problem Statement
Given weights and values of `n` items, put these items in a knapsack of capacity `W` to get the maximum total value in the knapsack. Each item can only be picked once.

## Approach / Intuition
For each item, we can either include it or exclude it. We use [[Dynamic Programming]] to store the maximum value achievable for all capacities up to `W`. We iterate backwards from `W` to avoid reusing the same item.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n * W)
- **[[Space Complexity]]:** O(W)

## Sample Code
```cpp
int knapSack(int W, vector<int>& wt, vector<int>& val, int n) {
    vector<int> dp(W + 1, 0);
    for (int i = 0; i < n; ++i) {
        for (int w = W; w >= wt[i]; --w) {
            dp[w] = max(dp[w], dp[w - wt[i]] + val[i]);
        }
    }
    return dp[W];
}
```

## New Keywords / STL Used
`max`, `vector`

## Edge Cases
Capacity `W` is 0, weights exceed capacity, single item.
