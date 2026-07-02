---
type: concept
tags: [matrix, array, cpp, toeplitz]
date: 2026-06-30
---
# Toeplitz Matrix

## Problem Statement
Given an $m \times n$ matrix, return `true` if the matrix is Toeplitz. Otherwise, return `false`.
A matrix is a **Toeplitz matrix** if every diagonal from top-left to bottom-right has the same elements.

---

## Approach: Top-Left Neighbor Comparison

A naive approach would be to extract every diagonal starting from the first row and first column, and verify if all elements in that diagonal match. However, this is tedious to implement.

A far simpler, elegant approach relies on a local property: in a valid Toeplitz matrix, every cell $M[i][j]$ must be identical to its top-left neighbor $M[i-1][j-1]$. 
By iterating through the matrix starting from row 1 and column 1, we can perform this $\mathcal{O}(1)$ check for each cell. If any cell violates the rule, we immediately return `false`.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

bool isToeplitzMatrix(vector<vector<int>>& matrix) {
    if (matrix.empty()) return true;
    
    int m = matrix.size();
    int n = matrix[0].size();
    
    // Start from row 1 and col 1 to ensure a valid top-left neighbor
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            if (matrix[i][j] != matrix[i - 1][j - 1]) {
                return false;
            }
        }
    }
    
    return true;
}
```

> [!tip]
> A matrix with a single row or single column trivially satisfies the Toeplitz condition, as the loop starting at `i = 1, j = 1` will immediately exit, correctly returning `true`.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(m \times n)$. We iterate through the matrix exactly once, performing a constant-time check.
- **Space Complexity:** $\mathcal{O}(1)$. The validation is done completely in-place.
