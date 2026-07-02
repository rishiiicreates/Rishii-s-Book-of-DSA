---
type: concept
tags: [matrix, array, cpp, boundary]
date: 2026-06-30
---
# Boundary Traversal

## Problem Statement
Given an $m \times n$ matrix, print the elements of its boundary in a clockwise manner starting from the top-left corner.

---

## Approach: Edge-by-Edge Traversal

The problem requires a fundamental matrix traversal cycle. We can break the boundary traversal into four distinct parts:
1. Top row: left to right.
2. Right column: top to bottom.
3. Bottom row: right to left.
4. Left column: bottom to top.

Special care must be taken for edge cases where the matrix consists of only one row or one column. In these cases, a full cycle is impossible and will lead to duplicate elements being printed or out-of-bounds errors.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

vector<int> boundaryTraversal(vector<vector<int>>& matrix) {
    if (matrix.empty()) return {};
    
    vector<int> res;
    int n = matrix.size();
    int m = matrix[0].size();
    
    // Edge case: single row
    if (n == 1) {
        for (int i = 0; i < m; i++) res.push_back(matrix[0][i]);
    } 
    // Edge case: single column
    else if (m == 1) {
        for (int i = 0; i < n; i++) res.push_back(matrix[i][0]);
    } 
    // General case
    else {
        // Top row
        for (int i = 0; i < m; i++) res.push_back(matrix[0][i]);
        // Right column
        for (int i = 1; i < n; i++) res.push_back(matrix[i][m - 1]);
        // Bottom row
        for (int i = m - 2; i >= 0; i--) res.push_back(matrix[n - 1][i]);
        // Left column
        for (int i = n - 2; i >= 1; i--) res.push_back(matrix[i][0]);
    }
    return res;
}
```

> [!tip]
> Notice the start and end indices of the loops. To avoid duplicate corner elements, the loop for the right column starts at `i = 1`, and the loop for the bottom row starts at `i = m - 2`.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(m + n)$. We traverse the perimeter of the matrix, which has approximately $2m + 2n$ elements.
- **Space Complexity:** $\mathcal{O}(1)$ auxiliary space. 
