---
type: concept
tags: [dp, cpp, stock]
date: 2026-06-30
---
# Buy and Sell - k Transactions

## Problem Statement
Find the maximum profit from buying and selling stocks. You may complete at most `k` transactions. You must sell the stock before you buy again.

## Approach / Intuition
Extend the state machine from 2 transactions to `k` transactions. Use a 2D [[Dynamic Programming]] array where `dp[i][0]` is the max profit holding a stock after `i` transactions, and `dp[i][1]` is not holding. If `k` is larger than `n/2`, it reduces to infinite transactions.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n * k)
- **[[Space Complexity]]:** O(k)

## Sample Code
```cpp
int maxProfit(int k, vector<int>& prices) {
    int n = prices.size();
    if (n == 0) return 0;
    if (k >= n / 2) {
        int profit = 0;
        for (int i = 1; i < n; ++i) {
            if (prices[i] > prices[i - 1]) profit += prices[i] - prices[i - 1];
        }
        return profit;
    }
    
    vector<int> buy(k + 1, -1e9);
    vector<int> sell(k + 1, 0);
    
    for (int p : prices) {
        for (int i = 1; i <= k; ++i) {
            buy[i] = max(buy[i], sell[i - 1] - p);
            sell[i] = max(sell[i], buy[i] + p);
        }
    }
    return sell[k];
}
```

## New Keywords / STL Used
`vector`, `max`

## Edge Cases
`k` is 0, empty array, `k` is very large.
