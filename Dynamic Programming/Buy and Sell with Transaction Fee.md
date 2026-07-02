---
type: concept
tags: [dp, cpp, stock]
date: 2026-06-30
---
# Buy and Sell with Transaction Fee

## Problem Statement
You are given an array of prices where `prices[i]` is the price of a given stock on the `i`-th day, and an integer `fee` representing a transaction fee. Find the maximum profit you can achieve. You may complete as many transactions as you like, but you must pay the fee for each transaction (buy + sell).

## Approach / Intuition
Use a [[State Machine]] logic utilizing [[Dynamic Programming]]. We track two states for each day: `hold` (having a stock) and `notHold` (not having a stock). We subtract the fee when buying (or selling). Space is optimized to O(1).

## Time & Space Complexity
- **[[Time Complexity]]:** O(n)
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
int maxProfit(vector<int>& prices, int fee) {
    if (prices.empty()) return 0;
    
    int hold = -prices[0];
    int notHold = 0;
    
    for (int i = 1; i < prices.size(); ++i) {
        int prevHold = hold;
        hold = max(hold, notHold - prices[i]);
        notHold = max(notHold, prevHold + prices[i] - fee);
    }
    
    return notHold;
}
```

## New Keywords / STL Used
`vector`, `max`

## Edge Cases
Empty array, fee is larger than any possible profit, constantly decreasing prices.
