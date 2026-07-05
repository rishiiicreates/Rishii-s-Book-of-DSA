# Weighted Climbing Stairs

## Problem Statement
- you are given an integer array `cost` where `cost[i]` is the cost of $i^{th}$ step on a staircase. Once you pay the cost, you can either climb one or two steps. Find the minimum cost to reach the top of the floor.

## Approach / Intuition
- we want to minimize the total cost. The minimum cost to reach step $i$ is `cost[i]` plus the minimum of the cost to reach step $i-1$ and step $i-2$. We use [[Dynamic Programming]] to calculate this iteratively from the bottom up, maintaining only the costs of the last two steps to optimize space.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$ where $N$ is the length of the `cost` array.
- **[[space Complexity]]:** $O(1)$ since we only store two previous values.

## Sample Code
```cpp
#include <vector>
#include <algorithm>

int minCostClimbingStairs(std::vector<int>& cost) {
    int n = cost.size();
    int prev2 = cost[0];
    int prev1 = cost[1];
    for (int i = 2; i < n; i++) {
        int curr = cost[i] + std::min(prev1, prev2);
        prev2 = prev1;
        prev1 = curr;
    }
    return std::min(prev1, prev2);
}
```

## New Keywords / STL Used
- `std::min`, `std::vector`

## Edge Cases
- array size of 2.

NEXT: [[Index]]
