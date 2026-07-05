# Min and Max Costs to Buy All Candies

## Problem Statement
- given an array representing the prices of candies and an integer K, find the minimum and maximum cost to buy all N candies, given that for every candy bought, you get K candies for free.

## Approach / Intuition
- sort the candy prices. For the minimum cost, buy the cheapest candies and take the most expensive ones for free. For the maximum cost, buy the most expensive candies and take the cheapest ones for free, using a [[Greedy Algorithm]].

## Time & Space Complexity
- **[[time Complexity]]:** O(N log N)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

pair<int, int> findMinMaxCost(vector<int>& candies, int K) {
    sort(candies.begin(), candies.end());
    int n = candies.size();
    int minCost = 0, maxCost = 0;
    int i = 0, j = n - 1;
    while (i <= j) {
        minCost += candies[i];
        i++;
        j -= K;
    }
    i = 0;
    j = n - 1;
    while (i <= j) {
        maxCost += candies[j];
        j--;
        i += K;
    }
    return {minCost, maxCost};
}
```

## New Keywords / STL Used
- `pair`

## Edge Cases
- k is 0
- k is larger than the number of candies

NEXT: [[Index]]
