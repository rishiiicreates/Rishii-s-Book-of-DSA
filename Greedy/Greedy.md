# Greedy Algorithm

## Problem Statement
- a general approach for solving optimization problems where we make the locally optimal choice at each step with the hope of finding a global optimum.

## Approach / Intuition
- the core idea of a [[Greedy Algorithm]] is to pick the best possible immediate choice. We assume that a local optimum will lead to a global optimum.

## Time & Space Complexity
- **[[time Complexity]]:** O(N log N) usually, due to sorting
- **[[space Complexity]]:** O(1) or O(N) depending on sorting

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
- all elements equal
- already sorted arrays

NEXT: [[Index]]
