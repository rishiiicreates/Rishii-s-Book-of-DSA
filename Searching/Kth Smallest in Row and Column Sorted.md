---
type: concept
tags: [searching, binary search, cpp, matrix]
date: 2026-06-30
---
# Kth Smallest in Row and Column Sorted Matrix

## Problem Statement
Given an $N \times N$ matrix where each of the rows and columns is sorted in strictly increasing order, find the $K$-th smallest element in the matrix.

*Example:* $K = 8$
$M = \begin{bmatrix} 1 & 5 & 9 \\ 10 & 11 & 13 \\ 12 & 13 & 15 \end{bmatrix}$
*Result:* $13$

---

## Approach: Binary Search with Staircase Traversal

Because the matrix is sorted row-wise and column-wise, a naive extraction takes $O(N^2 \log K)$. Using [[Binary Search]] on the value range coupled with a smart counting algorithm achieves $O(N \log(\text{Range}))$.

1. The minimum value is at $M[0][0]$ and the maximum is at $M[N-1][N-1]$. We binary search in this range $[L, R]$.
2. For a candidate value `mid`, we need to count how many elements are $\le$ `mid`.
3. **Staircase Traversal for Counting:** Start from the bottom-left corner ($r = N-1, c = 0$).
   - If $M[r][c] \le \text{mid}$, then every element above this cell in the same column is also $\le \text{mid}$ (since columns are sorted). We add `r + 1` to the count and move right: `c++`.
   - If $M[r][c] > \text{mid}$, this element is too large. We move up: `r--`.
4. If `count < K`, the $K$-th element must be larger: `low = mid + 1`.
5. If `count >= K`, `mid` is a valid candidate for the answer. We shrink the upper bound: `high = mid`.
6. Terminate when `low == high`.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

// Function to count elements <= target in the matrix
int countLessEqual(const vector<vector<int>>& matrix, int target, int n) {
    int count = 0;
    int row = n - 1;
    int col = 0;
    
    // Staircase traversal from bottom-left to top-right
    while (row >= 0 && col < n) {
        if (matrix[row][col] <= target) {
            count += (row + 1); // Entire column up to `row` is valid
            col++; // Move right
        } else {
            row--; // Move up
        }
    }
    return count;
}

int kthSmallest(const vector<vector<int>>& matrix, int k) {
    int n = matrix.size();
    int low = matrix[0][0];
    int high = matrix[n - 1][n - 1];
    
    while (low < high) {
        int mid = low + (high - low) / 2;
        
        if (countLessEqual(matrix, mid, n) < k) {
            low = mid + 1; // Kth smallest is strictly greater
        } else {
            high = mid; // Kth smallest is <= mid
        }
    }
    
    return low;
}
```

> [!tip]
> This staircase counting method operates in exactly $O(N)$ time because we move at most $N$ steps right and $N$ steps up. This dramatically outperforms calling `upper_bound` on each row, which would be $O(N \log N)$.

---

## Complexity Analysis
- **Time Complexity:** $O(N \log_2(\text{max} - \text{min}))$. The binary search runs $\log_2(\text{Range})$ times. The counting algorithm runs in $O(N)$ time.
- **Space Complexity:** $O(1)$. Traversal is fully in-place.
