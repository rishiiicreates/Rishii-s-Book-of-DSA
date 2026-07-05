# Matrix Zig-Zag (Row-Wise)

## Problem Statement
- given an $M \times N$ matrix, print the matrix in a row-wise zig-zag form.
- the first row is traversed left to right, the second row right to left, the third row left to right, and so forth, alternating direction identically to a snake game.


## Approach: Parity-Based Conditional Traversal

- the problem maps perfectly to the mathematical parity (even/odd state) of the row index.
- using 0-based indexing:
- row 0 (Even): Left to Right ($0 \to N-1$)
- row 1 (Odd): Right to Left ($N-1 \to 0$)
- row 2 (Even): Left to Right ($0 \to N-1$)

- algorithm:
- iterate an outer loop `i` from $0$ to $M-1$.
- check the parity using modulo operator: `i % 2`.
- if even, execute an inner loop `j` incrementing from $0$ to $N-1$.
- if odd, execute an inner loop `j` decrementing from $N-1$ down to $0$.


## Code Implementation

```cpp
#include <vector>
using namespace std;

vector<int> zigZagMatrix(vector<vector<int>>& matrix) {
    if (matrix.empty()) return {};
    
    int m = matrix.size();
    int n = matrix[0].size();
    
    vector<int> res;
    res.reserve(m * n);
    
    for (int i = 0; i < m; i++) {
        // Even Parity -> Traverse Left to Right
        if (i % 2 == 0) {
            for (int j = 0; j < n; j++) {
                res.push_back(matrix[i][j]);
            }
        } 
        // Odd Parity -> Traverse Right to Left
        else {
            for (int j = n - 1; j >= 0; j--) {
                res.push_back(matrix[i][j]);
            }
        }
    }
    
    return res;
}
```

> [!tip]
> Using `res.reserve(m * n)` prevents the `std::vector` from reallocating memory dynamically during the traversal, yielding a slight but consistent performance optimization.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(M \times N)$. Every single cell in the matrix is processed and appended exactly once.
- **space Complexity:** $\mathcal{O}(1)$ auxiliary space. The vector `res` strictly scales with $\mathcal{O}(M \times N)$ to hold the output format, but the algorithm relies on no external bounding constraints.

NEXT: [[Index]]
