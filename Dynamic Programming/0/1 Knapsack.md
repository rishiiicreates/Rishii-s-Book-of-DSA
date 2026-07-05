# 0/1 Knapsack

## Problem Statement
- given weights and values of `n` items, put these items in a knapsack of capacity `W` to get the maximum total value in the knapsack. Each item can only be picked once.

## Approach / Intuition
- for each item, we can either include it or exclude it. We use [[Dynamic Programming]] to store the maximum value achievable for all capacities up to `W`. We iterate backwards from `W` to avoid reusing the same item.

## Time & Space Complexity
- **[[time Complexity]]:** O(n * W)
- **[[space Complexity]]:** O(W)

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
- `max`, `vector`

## Edge Cases
- capacity `W` is 0, weights exceed capacity, single item.

NEXT: [[Index]]
