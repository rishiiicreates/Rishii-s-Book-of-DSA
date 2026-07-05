# Find a Peak Element II (2D Matrix)

## Problem Statement
- given an $M \times N$ matrix where no two adjacent cells are equal, find any peak element and return its coordinates `[r, c]`. An element is a peak if it is strictly greater than its top, bottom, left, and right neighbors. Assume the perimeter of the matrix is surrounded by $-1$.

- *example:*
- $m = \begin{bmatrix} 10 & 20 & 15 \\ 21 & 30 & 14 \\ 7 & 16 & 32 \end{bmatrix}$
- *result:* $[1, 1]$ (value $30$) or $[2, 2]$ (value $32$)


## Approach: Binary Search on Columns (Optimal)

- extending the logic from the 1D Peak finding problem, we can perform [[Binary Search]] on the columns (or rows).

- perform a binary search on the columns from `startCol = 0` to `endCol = N - 1`.
- find the middle column `midCol`.
- in this `midCol`, find the row index `maxRow` that contains the maximum element.
- compare $M[\text{maxRow}][\text{midCol}]$ with its left and right neighbors in the same row.
- if the element is smaller than its right neighbor, a peak *must* exist in the right half of the matrix. Set `startCol = midCol + 1`.
- if the element is smaller than its left neighbor, set `endCol = midCol - 1`.
- otherwise, if it is strictly greater than both (or if they are out of bounds), we have found a peak!


## Code Implementation

```cpp
#include <vector>
using namespace std;

vector<int> findPeakGrid(const vector<vector<int>>& mat) {
    int startCol = 0;
    int endCol = mat[0].size() - 1;
    
    while (startCol <= endCol) {
        int midCol = startCol + (endCol - startCol) / 2;
        
        // Find the maximum element in the current middle column
        int maxRow = 0;
        for (int i = 0; i < mat.size(); ++i) {
            if (mat[i][midCol] >= mat[maxRow][midCol]) {
                maxRow = i;
            }
        }
        
        // Safely check left and right neighbors
        bool leftIsBigger = (midCol - 1 >= startCol) && (mat[maxRow][midCol - 1] > mat[maxRow][midCol]);
        bool rightIsBigger = (midCol + 1 <= endCol) && (mat[maxRow][midCol + 1] > mat[maxRow][midCol]);
        
        if (!leftIsBigger && !rightIsBigger) {
            return {maxRow, midCol}; // Found the peak
        } else if (rightIsBigger) {
            startCol = midCol + 1; // Move right
        } else {
            endCol = midCol - 1; // Move left
        }
    }
    
    return {-1, -1};
}
```

> [!important]
> Why does finding the absolute maximum in the column guarantee a peak? Because the maximum element is mathematically guaranteed to be strictly greater than its top and bottom neighbors. Thus, we only need to check left and right to verify the peak property.


## Complexity Analysis
- **time Complexity:** $O(M \log_2 N)$. The binary search on columns takes $\log_2 N$ steps. Inside each step, we scan $M$ rows to find the maximum element.
- **space Complexity:** $O(1)$. No extra space is required.

NEXT: [[Index]]
