---
type: concept
tags: [backtracking, cpp, fundamentals]
date: 2026-06-30
---
# Backtracking

## Problem Statement
Understand the fundamental paradigm of backtracking for solving constraint satisfaction and exhaustive search problems.

## Approach / Intuition
[[Backtracking]] is an algorithmic technique for solving problems recursively by trying to build a solution incrementally. It involves exploring a path and, if it leads to a dead end or violates constraints, abandoning it and "backtracking" to the previous step. This is essentially a refined [[Depth-First Search]] (DFS) applied to a search space tree.

## Time & Space Complexity
- **[[Time Complexity]]:** Varies greatly, typically $O(b^d)$ where $b$ is the branching factor and $d$ is depth
- **[[Space Complexity]]:** $O(d)$ for the recursive call stack

## Sample Code
```cpp
#include <iostream>
#include <vector>

using namespace std;

void backtrack(int n, int current, vector<int>& path) {
    if (current == n) {
        for (int x : path) cout << x << " ";
        cout << "\n";
        return;
    }
    for (int i = 0; i < 2; i++) {
        path.push_back(i);
        backtrack(n, current + 1, path);
        path.pop_back();
    }
}
```

## New Keywords / STL Used
`pop_back`

## Edge Cases
Empty solution space, zero constraints.
