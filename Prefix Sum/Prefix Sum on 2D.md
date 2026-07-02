---
type: concept
tags: [prefix sum, cpp, matrix, 2d-array]
date: 2026-06-30
---
# Prefix Sum on 2D

## Problem Statement
Given a 2D matrix, compute a 2D prefix sum array where `prefix[i][j]` represents the sum of elements in the submatrix from `(0, 0)` to `(i, j)`.

## Approach / Intuition
To compute a [[2D Prefix Sum]], we use the principle of inclusion-exclusion. The sum at `(i, j)` includes the sum of the submatrix above it `(i-1, j)`, the submatrix to its left `(i, j-1)`, minus the overlapping top-left submatrix `(i-1, j-1)` which was added twice, plus the current element `matrix[i][j]`.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(R \times C)$
- **[[Space Complexity]]:** $O(R \times C)$

## Sample Code
```cpp
#include <vector>

std::vector<std::vector<int>> compute2DPrefixSum(std::vector<std::vector<int>>& matrix) {
    if (matrix.empty() || matrix[0].empty()) return {};
    
    int rows = matrix.size();
    int cols = matrix[0].size();
    std::vector<std::vector<int>> pref(rows, std::vector<int>(cols, 0));
    
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            pref[i][j] = matrix[i][j];
            if (i > 0) pref[i][j] += pref[i - 1][j];
            if (j > 0) pref[i][j] += pref[i][j - 1];
            if (i > 0 && j > 0) pref[i][j] -= pref[i - 1][j - 1];
        }
    }
    
    return pref;
}
```

## New Keywords / STL Used
2D `std::vector` initialization

## Edge Cases
- Empty matrix or empty rows
- Matrix with 1 row or 1 column
- Negative elements
