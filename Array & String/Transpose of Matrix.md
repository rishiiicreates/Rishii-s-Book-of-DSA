# Transpose of Matrix

## Problem Statement
- given a 2D integer array `matrix`, return the **transpose** of `matrix`.
- the transpose of a [[Matrix]] is the matrix flipped over its main diagonal, switching the row and column indices of the matrix.


## Approach: Dimensional Swap Allocation

- for an $m \times n$ matrix, its transpose will have dimensions $n \times m$.
- the fundamental transformation is:
$$ \text{transposed}[j][i] = \text{matrix}[i][j] $$

- if the matrix is square ($n \times n$), the transpose can be done **in-place** by swapping elements above the main diagonal with those below it. However, for a general $m \times n$ rectangular matrix, an in-place transpose is extremely complex. The standard optimal approach is to allocate a new $n \times m$ matrix and populate it according to the relation.


## Code Implementation

### General Approach ($m \times n$ Matrix)

```cpp
#include <vector>
using namespace std;

vector<vector<int>> transpose(vector<vector<int>>& matrix) {
    if (matrix.empty()) return {};
    
    int m = matrix.size();
    int n = matrix[0].size();
    
    // Allocate new matrix with swapped dimensions
    vector<vector<int>> res(n, vector<int>(m));
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            res[j][i] = matrix[i][j];
        }
    }
    
    return res;
}
```

### In-Place Transpose (Square $N \times N$ Matrix Only)

```cpp
#include <vector>
#include <algorithm>
using namespace std;

void transposeSquareInPlace(vector<vector<int>>& matrix) {
    int n = matrix.size();
    for (int i = 0; i < n; i++) {
        // Start j from i to only process the upper triangle
        for (int j = i + 1; j < n; j++) {
            swap(matrix[i][j], matrix[j][i]);
        }
    }
}
```

> [!warning]
> When writing the in-place version, ensure the inner loop starts at `j = i + 1`. If you start at `j = 0`, you will swap elements twice, completely undoing the transpose!


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(m \times n)$. We visit every cell of the matrix once.
- **space Complexity:** $\mathcal{O}(m \times n)$ for the generic out-of-place transformation, which requires creating a new matrix of swapped dimensions. The in-place square matrix approach achieves $\mathcal{O}(1)$ space.

NEXT: [[Index]]
