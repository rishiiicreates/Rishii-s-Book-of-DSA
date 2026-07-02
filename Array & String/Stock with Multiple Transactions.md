---
type: concept
tags: [array, cpp, greedy]
date: 2026-06-30
---
# Best Time to Buy and Sell Stock (Multiple Transactions)

## Problem Statement
Given an array `prices` of size $N$ where `prices[i]` is the price of a given stock on the $i$-th day. You can perform an unlimited number of transactions (buy one and sell one share of the stock multiple times). You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again). Find the maximum profit.

*Example:* `prices` $= [7, 1, 5, 3, 6, 4]$
*Result:* $7$ (Buy at 1, sell at 5 -> profit $4$. Buy at 3, sell at 6 -> profit $3$. Total = $7$)

---

## Approach: Peak-Valley / Greedy Sum (Optimal)

This is a classic [[Greedy Algorithm]] problem. The insight is that any upward trend in the stock graph can be captured by summing the positive differences between consecutive days. 

If prices go from $1 \rightarrow 5 \rightarrow 6$, buying at 1 and selling at 6 gives $5$. However, buying at 1 and selling at 5 ($+4$), then immediately buying at 5 and selling at 6 ($+1$), also yields $5$.
Thus, we don't need to actually find the local minima and maxima. We simply add the profit for every single day where $P_i > P_{i-1}$.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

int maxProfitMultiple(const vector<int>& prices) {
    int n = prices.size();
    int profit = 0;
    
    for (int i = 1; i < n; ++i) {
        // If today's price is strictly greater than yesterday's, harvest the profit.
        if (prices[i] > prices[i - 1]) {
            profit += (prices[i] - prices[i - 1]);
        }
    }
    
    return profit;
}
```

> [!important]
> Notice that we iterate starting from `i = 1`. Comparing `prices[i]` with `prices[i - 1]` inherently avoids out-of-bounds errors on the left side of the array.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ where $N$ is the number of days. We traverse the price array exactly once.
- **Space Complexity:** $O(1)$. The profit accumulator is the only extra memory needed.
