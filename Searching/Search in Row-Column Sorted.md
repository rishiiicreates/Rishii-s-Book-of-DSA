# Search in Row-Column Sorted Matrix

## Problem Statement
- given an $M \times N$ integer matrix where every row is sorted in strictly increasing order from left to right, and every column is sorted in strictly increasing order from top to bottom, determine if a target $T$ exists in the matrix.

- *example:* $T = 5$
- $m = \begin{bmatrix} 1 & 4 & 7 & 11 \\ 2 & 5 & 8 & 12 \\ 3 & 6 & 9 & 16 \\ 10 & 13 & 14 & 17 \end{bmatrix}$
- *result:* `true` (Found at $[1, 1]$)


## Approach: Top-Right Pointer Reduction

- a brute-force search is $O(M \times N)$. A row-by-row binary search is $O(M \log N)$.
- we can achieve $O(M + N)$ by exploiting the grid structure, similar to a 2D [[Two Pointers]] method.

- start at the top-right corner of the matrix: $r = 0, c = N - 1$.
- if $M[r][c] == T$, return `true`.
- if $M[r][c] > T$, the target cannot be in the current column (everything below is larger). Thus, we move left: `c--`.
- if $M[r][c] < T$, the target cannot be in the current row (everything to the left is smaller). Thus, we move down: `r++`.
- repeat until out of bounds.


## Code Implementation

```cpp
#include <vector>
using namespace std;

bool searchMatrixRowColSorted(const vector<vector<int>>& matrix, int target) {
    if (matrix.empty() || matrix[0].empty()) return false;
    
    int m = matrix.size();
    int n = matrix[0].size();
    
    // Start at the top-right corner
    int r = 0;
    int c = n - 1;
    
    while (r < m && c >= 0) {
        if (matrix[r][c] == target) {
            return true;
        } else if (matrix[r][c] > target) {
            // Target must be strictly to the left
            c--;
        } else {
            // Target must be strictly below
            r++;
        }
    }
    
    return false;
}
```

> [!tip]
> You could symmetrically start at the **bottom-left** corner ($r = M-1, c = 0$). If $M[r][c] > T$, move up (`r--`); if $M[r][c] < T$, move right (`c++`). Both corners work perfectly because they act as saddle points (one dimension increases, the other decreases). You *cannot* start at top-left or bottom-right.


## Complexity Analysis
- **time Complexity:** $O(M + N)$. In the worst case, we traverse from the top-right all the way to the bottom-left, taking at most $M$ downward steps and $N$ leftward steps.
- **space Complexity:** $O(1)$. The traversal is strictly in-place.

NEXT: [[Index]]
