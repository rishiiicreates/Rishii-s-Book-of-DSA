---
type: concept
tags: [matrix, array, cpp, prefix-sum, combinatorics]
date: 2026-07-01
---
# Range Sum Query 2D (Queries in a Matrix)

## Problem Statement
Given a 2D matrix, efficiently calculate the sum of the elements inside an arbitrary rectangle defined by its upper-left corner $(R_1, C_1)$ and lower-right corner $(R_2, C_2)$. The algorithm should handle an immense number of queries.

---

## Approach: 2D Prefix Sum Array

If we answer queries by manually iterating through the matrix, each query takes $O(M \times N)$ time.
To reduce the query time to $O(1)$, we use a 2D **Prefix Sum** matrix.

Let $P[i][j]$ represent the sum of all elements in the rectangular region bounded by the origin $(0,0)$ and the corner $(i-1, j-1)$. (We use a 1-based index prefix matrix to seamlessly handle boundary edge cases).

**1. Building the Prefix Matrix:**
Using the Inclusion-Exclusion Principle, we compute $P[i][j]$ dynamically:
$$ P[i][j] = A[i-1][j-1] + P[i-1][j] + P[i][j-1] - P[i-1][j-1] $$
- We add the matrix element $A[i-1][j-1]$.
- We add the sum from the rectangle above ($P[i-1][j]$).
- We add the sum from the rectangle to the left ($P[i][j-1]$).
- Since the top-left overlapping rectangle ($P[i-1][j-1]$) was added twice, we subtract it once.

**2. Querying a Region:**
To find the sum of the region bounded by $(R_1, C_1)$ to $(R_2, C_2)$:
$$ \text{Sum} = P[R_2+1][C_2+1] - P[R_1][C_2+1] - P[R_2+1][C_1] + P[R_1][C_1] $$
- Take the total area down to the bottom-right corner.
- Subtract the area strictly above the query rectangle.
- Subtract the area strictly to the left of the query rectangle.
- Add back the top-left overlapping area, which was erroneously subtracted twice.

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

class NumMatrix {
private:
    vector<vector<int>> prefix;

public:
    NumMatrix(vector<vector<int>>& matrix) {
        if (matrix.empty() || matrix[0].empty()) return;
        
        int m = matrix.size();
        int n = matrix[0].size();
        
        // 1-based indexing for the prefix sum matrix
        prefix.assign(m + 1, vector<int>(n + 1, 0));
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                prefix[i][j] = matrix[i - 1][j - 1] 
                             + prefix[i - 1][j] 
                             + prefix[i][j - 1] 
                             - prefix[i - 1][j - 1];
            }
        }
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        return prefix[row2 + 1][col2 + 1] 
             - prefix[row1][col2 + 1] 
             - prefix[row2 + 1][col1] 
             + prefix[row1][col1];
    }
};
```

---

## Complexity Analysis
- **Time Complexity:** 
  - **Construction:** $O(M \times N)$ to precompute the prefix matrix.
  - **Query:** $O(1)$ strictly constant time math operations.
- **Space Complexity:** $O(M \times N)$ memory to store the `prefix` matrix.

> [!tip]
> Using $M+1 \times N+1$ dimensions for the prefix sum is an elegant combinatorial trick. It completely eradicates the need for bounds checking (`if row1 == 0` or `if col1 == 0`), making the query function branchless and incredibly fast.
