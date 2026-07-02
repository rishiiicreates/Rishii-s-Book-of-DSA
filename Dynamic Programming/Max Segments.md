---
type: concept
tags: [dp, cpp, knapsack_variation]
date: 2026-06-30
---
# Max Segments

## Problem Statement
Given a line segment of length $N$, cut it into the maximum number of segments such that the length of each segment is either $X$, $Y$, or $Z$.

## Approach / Intuition
This is an [[Unbounded Knapsack]] variation. Let `dp[i]` be the maximum segments for a rod of length $i$. To find `dp[i]`, we look at `dp[i-X]`, `dp[i-Y]`, and `dp[i-Z]` and take the maximum among valid previous states, then add 1. We initialize the DP array with a very small negative value and `dp[0] = 0` to denote invalid states.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(N)$ for the DP array.

## Sample Code
```cpp
#include <vector>
#include <algorithm>

int maximizeTheCuts(int n, int x, int y, int z) {
    std::vector<int> dp(n + 1, -1e9);
    dp[0] = 0;
    for (int i = 1; i <= n; i++) {
        if (i - x >= 0) dp[i] = std::max(dp[i], dp[i - x] + 1);
        if (i - y >= 0) dp[i] = std::max(dp[i], dp[i - y] + 1);
        if (i - z >= 0) dp[i] = std::max(dp[i], dp[i - z] + 1);
    }
    return dp[n] < 0 ? 0 : dp[n];
}
```

## New Keywords / STL Used
`std::vector`, `std::max`

## Edge Cases
Impossible to cut exactly to $N$ (returns 0), $X, Y, Z > N$.
