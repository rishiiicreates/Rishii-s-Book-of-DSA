# Coin Change II

## Problem Statement
- given an integer array of coins and an integer amount, return the number of combinations that make up that amount. You have an infinite number of each kind of coin.

## Approach / Intuition
- this is a variation of the [[Unbounded Knapsack]] problem. We use a 1D [[Dynamic Programming]] array where `dp[i]` stores the number of ways to make amount `i`. We iterate forwards since we can reuse coins multiple times.

## Time & Space Complexity
- **[[time Complexity]]:** O(n * amount)
- **[[space Complexity]]:** O(amount)

## Sample Code
```cpp
int change(int amount, vector<int>& coins) {
    vector<int> dp(amount + 1, 0);
    dp[0] = 1;
    for (int coin : coins) {
        for (int i = coin; i <= amount; ++i) {
            dp[i] += dp[i - coin];
        }
    }
    return dp[amount];
}
```

## New Keywords / STL Used
- `vector`

## Edge Cases
- amount is 0 (1 way: empty set), coins array is empty, amount impossible to make.

NEXT: [[Index]]
