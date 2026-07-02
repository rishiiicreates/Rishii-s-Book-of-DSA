---
type: concept
tags: [matrix, array, cpp, simulation, combinatorics]
date: 2026-06-30
---
# Magic Square

## Problem Statement
Given an $R \times C$ grid of integers, find the number of $3 \times 3$ magic square subgrids.
A $3 \times 3$ magic square is a $3 \times 3$ grid filled with distinct numbers from **1 to 9** such that each row, column, and both diagonals all have the exact same sum.

---

## Approach: Combinatorial Pruning & Grid Simulation

A brute force approach would inspect every $3 \times 3$ subgrid and verify 8 separate sums (3 rows, 3 columns, 2 diagonals) and uniqueness.

However, we can heavily prune the search space utilizing the strict mathematical properties of a $3 \times 3$ magic square constructed from digits 1 to 9:
1. **The Magic Constant:** The sum of digits 1-9 is 45. Since there are 3 rows, each row must mathematically sum to exactly $45 / 3 = 15$. 
2. **The Center Cell:** The center cell belongs to one row, one column, and both diagonals. The only mathematical way to arrange the digits 1-9 such that all symmetric lines cross the center and sum to 15 is if the **center cell is strictly 5**.

Algorithm:
1. Iterate through all possible top-left corners of a $3 \times 3$ subgrid.
2. Pruning: If the center cell (`grid[r+1][c+1]`) is not 5, skip immediately.
3. Verification: Check if the grid contains all digits 1-9 exactly once.
4. Validation: Verify that all rows, columns, and diagonals sum to 15.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

bool isMagic(const vector<vector<int>>& grid, int r, int c) {
    // Mathematical Pruning: Center must be 5
    if (grid[r + 1][c + 1] != 5) return false;
    
    // Check for distinct digits 1 to 9
    vector<int> count(10, 0);
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            int val = grid[r + i][c + j];
            if (val < 1 || val > 9 || count[val] > 0) return false;
            count[val]++;
        }
    }
    
    // If uniqueness and bounds hold, verify the 15-sum condition
    // Diagonals (Center is already confirmed as 5)
    if (grid[r][c] + grid[r+2][c+2] != 10) return false;
    if (grid[r][c+2] + grid[r+2][c] != 10) return false;
    
    // Rows
    if (grid[r][c] + grid[r][c+1] + grid[r][c+2] != 15) return false;
    if (grid[r+2][c] + grid[r+2][c+1] + grid[r+2][c+2] != 15) return false;
    
    // Columns
    if (grid[r][c] + grid[r+1][c] + grid[r+2][c] != 15) return false;
    if (grid[r][c+2] + grid[r+1][c+2] + grid[r+2][c+2] != 15) return false;
    
    return true;
}

int numMagicSquaresInside(vector<vector<int>>& grid) {
    int count = 0;
    int rows = grid.size();
    int cols = grid[0].size();
    
    // A magic square requires at least a 3x3 dimension
    if (rows < 3 || cols < 3) return 0;
    
    // Iterate through top-left corners of every possible 3x3 subgrid
    for (int i = 0; i <= rows - 3; i++) {
        for (int j = 0; j <= cols - 3; j++) {
            if (isMagic(grid, i, j)) {
                count++;
            }
        }
    }
    
    return count;
}
```

> [!important]
> Because we proved the center is 5, we only need to verify that the opposite corners sum to 10 (`15 - 5`), saving redundant addition operations. Furthermore, the middle row and middle column are mathematically guaranteed to be correct if the top/bottom rows and left/right columns evaluate to 15, allowing us to skip them entirely!

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(R \times C)$, where $R$ and $C$ are the dimensions of the grid. For each valid cell, the inner verification runs in strict $\mathcal{O}(1)$ time (at most 9 operations).
- **Space Complexity:** $\mathcal{O}(1)$ auxiliary space, since we only use a fixed 10-element frequency array.
