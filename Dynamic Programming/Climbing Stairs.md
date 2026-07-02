---
type: concept
tags: [dp, cpp, linear_dp]
date: 2026-06-30
---
# Climbing Stairs

## Problem Statement
Given $n$ steps, you can climb either 1 or 2 steps at a time. Find the total number of distinct ways to reach the top.

## Approach / Intuition
To reach step $i$, you can come from either step $i-1$ (taking 1 step) or step $i-2$ (taking 2 steps). Thus, the number of ways to reach step $i$ is the sum of ways to reach $i-1$ and $i-2$, similar to the [[Fibonacci]] sequence. This overlapping subproblem structure makes it ideal for [[Dynamic Programming]]. We can optimize space by only keeping the last two results.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$ to compute the ways for $N$ steps.
- **[[Space Complexity]]:** $O(1)$ using space optimization, or $O(N)$ with a DP array.

## Sample Code
```cpp
int climbStairs(int n) {
    if (n <= 2) return n;
    int prev2 = 1, prev1 = 2;
    for (int i = 3; i <= n; ++i) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

## New Keywords / STL Used
None specifically, simple variables.

## Edge Cases
$n = 0$, $n = 1$, $n = 2$.
