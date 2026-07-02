---
type: concept
tags: [dp, cpp, general]
date: 2026-06-30
---
# Dynamic Programming

## Problem Statement
[[Dynamic Programming]] is a technique to solve optimization problems by breaking them down into simpler subproblems, solving each subproblem exactly once, and storing their solutions to avoid redundant computations.

## Approach / Intuition
The core idea of [[Dynamic Programming]] is to trade space for time. It can be implemented using either a top-down approach ([[Memoization]]) which uses [[Recursion]], or a bottom-up approach ([[Tabulation]]) which builds the solution iteratively. We need to identify optimal substructure and overlapping subproblems.

## Time & Space Complexity
- **[[Time Complexity]]:** Usually equal to the number of states multiplied by the time taken to transition between states.
- **[[Space Complexity]]:** Typically $O(N)$ or $O(N \times M)$ depending on the state dimensions, often optimizable to $O(1)$ or $O(\text{min}(N, M))$ using space optimization.

## Sample Code
```cpp

```

## New Keywords / STL Used
`std::vector` for DP tables, `std::max`, `std::min`, `memset` for initialization.

## Edge Cases
Empty inputs, base cases not properly initialized.
