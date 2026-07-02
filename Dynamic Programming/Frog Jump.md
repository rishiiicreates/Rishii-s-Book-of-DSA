---
type: concept
tags: [dp, cpp, min_cost]
date: 2026-06-30
---
# Frog Jump

## Problem Statement
A frog is on the $1^{st}$ step of an $N$ stairs long staircase. The frog wants to reach the $N^{th}$ stair. The energy lost in jumping from $i^{th}$ stair to $j^{th}$ stair is $|height[i] - height[j]|$. The frog can jump 1 or 2 steps. Find the minimum energy required to reach the last stair.

## Approach / Intuition
This is similar to climbing stairs but with dynamic costs based on the height difference. The state `dp[i]` represents the minimum energy to reach step $i$. We can reach $i$ from $i-1$ or $i-2$. We calculate the minimum of these two possibilities. Using [[Dynamic Programming]] with space optimization reduces the memory requirement.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(1)$

## Sample Code
```cpp
#include <vector>
#include <cmath>
#include <algorithm>
#include <climits>

int frogJump(int n, std::vector<int> &heights) {
    int prev2 = 0;
    int prev1 = 0;
    for (int i = 1; i < n; i++) {
        int jumpTwo = INT_MAX;
        int jumpOne = prev1 + std::abs(heights[i] - heights[i - 1]);
        if (i > 1) {
            jumpTwo = prev2 + std::abs(heights[i] - heights[i - 2]);
        }
        int curr = std::min(jumpOne, jumpTwo);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

## New Keywords / STL Used
`std::abs`, `INT_MAX`, `std::min`

## Edge Cases
$n = 1$, heights array containing identical values.
