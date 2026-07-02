---
type: concept
tags: [sorting, cpp, greedy-cost]
date: 2026-06-30
---
# Candy Cost

## Problem Statement
Given an array $P$ representing the prices of $N$ candies. There is a store promotion: for every $1$ candy you buy, you can get $K$ other candies for free. Find the minimum cost to acquire all $N$ candies.

*Example:* $P = [3, 2, 1, 4]$, $K = 2$
*Result:* $3$ (Buy the $3$ candy, get $4$ and $2$ for free. Buy the $1$ candy, get nothing for free. Total $= 3+1 = 4$. Wait, the minimum cost is actually to buy $2$, get $4$ and $3$ for free, then buy $1$. Total $= 2 + 1 = 3$.)

*Correction to Intuition:* To minimize the total cost, we should always buy the cheapest candies and use the free tokens to acquire the most expensive candies. 

---

## Approach: Greedy Sorting

We want to pay for the cheapest candies and take the most expensive ones for free.

1. Sort the prices array $P$ in ascending order.
2. We start buying from the beginning (cheapest).
3. For each purchase at index `i`, we pay $P[i]$.
4. The promotion allows us to eliminate $K$ of the most expensive remaining candies. So, we dynamically reduce the boundary of available candies by $K$ from the back.
5. We repeat this until the front pointer `i` surpasses the active boundary.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

int minCost(vector<int>& prices, int k) {
    sort(prices.begin(), prices.end());
    
    int cost = 0;
    int n = prices.size();
    
    for (int i = 0; i < n; i++) {
        // Buy the cheapest available candy
        cost += prices[i];
        
        // Take the K most expensive available candies for free
        n -= k; 
    }
    
    return cost;
}
```

> [!important]
> The pointer `n` acts as the upper boundary for candies that still need to be acquired. Every time we buy one, we effectively cross off $K$ items from the right end of the sorted array.

---

## Complexity Analysis
- **Time Complexity:** $O(N \log N)$. Sorting the array dominates the runtime. The greedy accumulation takes $O(N/K)$ steps.
- **Space Complexity:** $O(1)$ auxiliary space, excluding sorting overhead.
