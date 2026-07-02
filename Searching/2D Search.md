---
type: concept
tags: [searching, binary-search, cpp, matrix, math]
date: 2026-06-30
---
# 2D Search

## Problem Statement
Search for a `target` value in an $M \times N$ integer matrix. The matrix possesses the following mathematical properties:
1. Each row is sorted strictly in ascending order from left to right.
2. The first integer of each row is strictly greater than the last integer of the previous row.

---

## Approach: 1D Flattening Isomorphism

The two mathematical properties guarantee that if we concatenate all the rows of the matrix, the resulting 1D sequence of length $M \times N$ will be strictly monotonically increasing.
Instead of actually creating this flattened array (which requires $\mathcal{O}(M \times N)$ auxiliary space), we can simulate it virtually utilizing a continuous index mapping.

A generic binary search operates on a 1D index line from $0$ to $M \times N - 1$.
Given any virtual 1D index `mid`, its corresponding 2D coordinates `(row, col)` in an $M \times N$ matrix are mathematically defined as:
- `row` = $\lfloor \text{mid} / N \rfloor$
- `col` = $\text{mid} \pmod N$

By substituting this coordinate mapping into the standard binary search template, the 2D matrix operates exactly like a virtual 1D sorted array.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

bool searchMatrix(const vector<vector<int>>& matrix, int target) {
    if (matrix.empty() || matrix[0].empty()) return false;
    
    int m = matrix.size();
    int n = matrix[0].size();
    
    int left = 0;
    int right = m * n - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        // Isomorphic mapping from 1D 'mid' to 2D coordinates
        int midVal = matrix[mid / n][mid % n];
        
        if (midVal == target) {
            return true;
        } 
        else if (midVal < target) {
            left = mid + 1;
        } 
        else {
            right = mid - 1;
        }
    }
    
    return false;
}
```

> [!important]
> The division and modulo operations must specifically use $N$ (the number of columns, `matrix[0].size()`), **not** $M$ (the number of rows). Using $M$ is a fatal mathematical error that breaks the coordinate projection.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(\log(M \times N))$. Since $\log(M \times N) = \log(M) + \log(N)$, the search space is logarithmically halved at every iteration.
- **Space Complexity:** $\mathcal{O}(1)$. The algorithm requires strictly a few integer pointers to simulate the virtual mapping.
