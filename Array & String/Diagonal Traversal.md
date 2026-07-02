---
type: concept
tags: [matrix, array, cpp, diagonal]
date: 2026-06-30
---
# Diagonal Traversal

## Problem Statement
Given an `m x n` matrix, return an array of all the elements of the matrix in a diagonal order.

---

## Approach: Directional Flag & Boundary Checks

The core intuition is to traverse the matrix diagonally by maintaining a direction flag. We alternate between moving up-right and down-left. Boundary conditions require shifting the row or column index when they go out of bounds. The sum of coordinates $r + c$ identifies each diagonal line, which is a classic matrix traversal trick.

For an element at $(r, c)$:
- If $(r + c)$ is even, the traversal direction is up-right ($r \to r-1$, $c \to c+1$).
- If $(r + c)$ is odd, the traversal direction is down-left ($r \to r+1$, $c \to c-1$).

We must carefully handle the boundary conditions when $r$ or $c$ go out of bounds to correctly transition to the next diagonal.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

vector<int> findDiagonalOrder(vector<vector<int>>& mat) {
    if (mat.empty()) return {};
    
    int m = mat.size();
    int n = mat[0].size();
    vector<int> res;
    res.reserve(m * n);
    
    int r = 0, c = 0;
    
    for (int i = 0; i < m * n; i++) {
        res.push_back(mat[r][c]);
        
        // Even diagonal: moving up-right
        if ((r + c) % 2 == 0) {
            if (c == n - 1) { 
                r++; // Hit right boundary, move down
            } else if (r == 0) { 
                c++; // Hit top boundary, move right
            } else { 
                r--; 
                c++; 
            }
        } 
        // Odd diagonal: moving down-left
        else {
            if (r == m - 1) { 
                c++; // Hit bottom boundary, move right
            } else if (c == 0) { 
                r++; // Hit left boundary, move down
            } else { 
                r++; 
                c--; 
            }
        }
    }
    return res;
}
```

> [!important] 
> The order of boundary checks is extremely crucial. For example, when moving up-right, if we hit the top-right corner, we are hitting both the top and right boundaries. We must check the right boundary (`c == n - 1`) first to correctly increment `r` instead of incrementing `c` and going out of bounds.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(m \times n)$. We visit each element of the $m \times n$ matrix exactly once.
- **Space Complexity:** $\mathcal{O}(1)$ auxiliary space. The result vector takes $\mathcal{O}(m \times n)$ space, but returning the answer usually doesn't count towards auxiliary space complexity.
