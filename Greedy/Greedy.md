---
type: concept
tags: [greedy, cpp, greedy-algorithm]
date: 2026-06-30
---
# Greedy Algorithm

## Problem Statement
A general approach for solving optimization problems where we make the locally optimal choice at each step with the hope of finding a global optimum.

## Approach / Intuition
The core idea of a [[Greedy Algorithm]] is to pick the best possible immediate choice. We assume that a local optimum will lead to a global optimum.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log N) usually, due to sorting
- **[[Space Complexity]]:** O(1) or O(N) depending on sorting

## Sample Code
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

void greedyExample(vector<int>& items) {
    sort(items.begin(), items.end(), greater<int>());
    int currentSum = 0;
    for (int item : items) {
        currentSum += item;
    }
}
```

## New Keywords / STL Used
- `sort` with `greater<int>()`

## Edge Cases
- All elements equal
- Already sorted arrays
