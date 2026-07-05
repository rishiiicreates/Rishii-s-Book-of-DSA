# 0-1 Knapsack

## Problem Statement
- given a set of items, each with a weight and a value, determine the maximum value you can pack in a knapsack of capacity `W`. Each item can be picked at most once (0-1).

## Approach / Intuition
- this is the classic [[Dynamic Programming]] formulation where we must decide to either include or exclude each item. We define a state `dp[w]` representing the maximum value attainable with capacity `w`. We iterate through the items and for each item, traverse the capacities backwards (from `W` down to the item's weight) to ensure we use each item exactly once. This reduces the two-dimensional state space to a highly efficient one-dimensional array.

## Time & Space Complexity
- **[[time Complexity]]:** O(N * W)
- **[[space Complexity]]:** O(W)

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

int knapSack(int W, vector<int>& wt, vector<int>& val, int n) {
    vector<int> dp(W + 1, 0);
    
    for (int i = 0; i < n; i++) {
        for (int w = W; w >= wt[i]; w--) {
            dp[w] = max(dp[w], dp[w - wt[i]] + val[i]);
        }
    }
    
    return dp[W];
}
```

## New Keywords / STL Used
- `std::vector`

## Edge Cases
- knapsack capacity `W = 0`
- all items have weights greater than `W`
- some items have value 0

NEXT: [[Index]]
