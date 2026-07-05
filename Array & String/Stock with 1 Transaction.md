# Best Time to Buy and Sell Stock (1 Transaction)

## Problem Statement
- given an array `prices` of size $N$ where `prices[i]` is the price of a given stock on the $i$-th day. You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock. Return the maximum profit. If no profit can be made, return $0$.

- *example:* `prices` $= [7, 1, 5, 3, 6, 4]$
- *result:* $5$ (Buy on day 2 at price 1, sell on day 5 at price 6)


## Approach: Dynamic Minimum (Optimal)

- to maximize the profit, we need to minimize the buying price and maximize the selling price. However, the sell day must strictly follow the buy day.
- this naturally forms a [[Greedy Algorithm]] where, at any day $i$, the optimal buying day would be the one with the *minimum price seen so far*.

- track `minPrice` initialized to infinity.
- track `maxProfit` initialized to $0$.
- as you iterate day by day, update `minPrice`.
- the potential profit on day $i$ is `prices[i] - minPrice`. Maximize `maxProfit` with this value.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>
#include <climits>
using namespace std;

int maxProfit(const vector<int>& prices) {
    int minPrice = INT_MAX;
    int maxProf = 0;
    
    for (int price : prices) {
        minPrice = min(minPrice, price);
        maxProf = max(maxProf, price - minPrice);
    }
    
    return maxProf;
}
```

> [!tip]
> Using `INT_MAX` from `<climits>` is a safe way to initialize minimum trackers. This ensures that the very first element will instantly overwrite it.

> [!warning]
> If the array is strictly decreasing (e.g., $[7, 6, 4, 3, 1]$), the maximum profit is technically negative, but since we have the choice to *not* transact, we should return $0$. The initialization `maxProf = 0` handles this perfectly.


## Complexity Analysis
- **time Complexity:** $O(N)$ where $N$ is the number of days. We perform a single pass.
- **space Complexity:** $O(1)$. We only track two integer variables.

NEXT: [[Index]]
