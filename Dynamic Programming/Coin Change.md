---
type: concept
tags: [dp, cpp, knapsack]
date: 2026-06-30
---
# Coin Change

## Problem Statement
Given an array of coins representing different denominations and an integer amount, return the fewest number of coins that you need to make up that amount. If it cannot be made, return -1.

## Approach / Intuition
This problem maps to the [[Unbounded Knapsack]] concept. We use an array `dp` where `dp[i]` represents the minimum coins needed for amount $i$. For each coin denomination and for each amount from the coin's value up to the target amount, we transition using `dp[i] = min(dp[i], dp[i - coin] + 1)`.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(A \times C)$ where $A$ is the amount and $C$ is the number of coins.
- **[[Space Complexity]]:** $O(A)$ for the DP array.

## Sample Code
```cpp
#include <vector>
#include <algorithm>

int coinChange(std::vector<int>& coins, int amount) {
    std::vector<int> dp(amount + 1, amount + 1);
    dp[0] = 0;
    for (int i = 1; i <= amount; i++) {
        for (int coin : coins) {
            if (i - coin >= 0) {
                dp[i] = std::min(dp[i], dp[i - coin] + 1);
            }
        }
    }
    return dp[amount] > amount ? -1 : dp[amount];
}
```

## New Keywords / STL Used
`std::vector`, `std::min`

## Edge Cases
Amount is 0, impossible to form amount, large coin denominations.
