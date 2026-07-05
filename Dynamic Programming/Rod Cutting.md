# Rod Cutting

## Problem Statement
- given a rod of length `n` and an array of prices that contains prices of all pieces of size smaller than `n`. Determine the maximum value obtainable by cutting up the rod and selling the pieces.

## Approach / Intuition
- this can be modeled as the [[Unbounded Knapsack]] problem where weights are the lengths `1` to `n` and capacities are up to `n`. We use a 1D [[Dynamic Programming]] array, taking the maximum of skipping the current cut length or taking it and reducing the rod length.

## Time & Space Complexity
- **[[time Complexity]]:** O(n^2)
- **[[space Complexity]]:** O(n)

## Sample Code
```cpp
int cutRod(vector<int>& price, int n) {
    vector<int> dp(n + 1, 0);
    for (int i = 1; i <= n; ++i) {
        for (int j = i; j <= n; ++j) {
            dp[j] = max(dp[j], price[i - 1] + dp[j - i]);
        }
    }
    return dp[n];
}
```

## New Keywords / STL Used
- `max`, `vector`

## Edge Cases
- rod length is 0, prices are 0, max value comes from not cutting at all.

NEXT: [[Index]]
