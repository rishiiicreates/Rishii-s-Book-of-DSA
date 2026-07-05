# Matrix Spiral

## Problem Statement
- given an $m \times n$ matrix, return all elements of the matrix in spiral order.


## Approach: Layer-by-Layer Shrinking Boundaries

- the most intuitive way to traverse a matrix in a spiral is to simulate the process by defining four boundary pointers: `top`, `bottom`, `left`, and `right`.

- we iterate inward, collecting elements:
- traverse from `left` to `right` across the `top` row. Increment `top`.
- traverse from `top` to `bottom` down the `right` column. Decrement `right`.
- traverse from `right` to `left` across the `bottom` row. Decrement `bottom`.
- traverse from `bottom` to `top` up the `left` column. Increment `left`.

- we must constantly check if the boundaries have crossed (`top <= bottom` and `left <= right`) before traversing the bottom row and left column. This is a common pitfall that leads to duplicates in rectangular matrices.


## Code Implementation

```cpp
#include <vector>
using namespace std;

vector<int> spiralOrder(vector<vector<int>>& matrix) {
    if (matrix.empty()) return {};
    
    vector<int> res;
    int top = 0, bottom = matrix.size() - 1;
    int left = 0, right = matrix[0].size() - 1;
    
    while (top <= bottom && left <= right) {
        // Traverse Right
        for (int i = left; i <= right; i++) 
            res.push_back(matrix[top][i]);
        top++;
        
        // Traverse Down
        for (int i = top; i <= bottom; i++) 
            res.push_back(matrix[i][right]);
        right--;
        
        // Traverse Left
        if (top <= bottom) {
            for (int i = right; i >= left; i--) 
                res.push_back(matrix[bottom][i]);
            bottom--;
        }
        
        // Traverse Up
        if (left <= right) {
            for (int i = bottom; i >= top; i--) 
                res.push_back(matrix[i][left]);
            left++;
        }
    }
    
    return res;
}
```

> [!important]
> The checks `if (top <= bottom)` and `if (left <= right)` inside the loop are crucial for non-square matrices (e.g., $3 \times 5$). Without them, you will process the remaining inner row or column twice, duplicating elements at the center of the spiral!


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(m \times n)$. Every single element is visited and added to our result array exactly once.
- **space Complexity:** $\mathcal{O}(1)$ auxiliary space, excluding the output array.

NEXT: [[Index]]
