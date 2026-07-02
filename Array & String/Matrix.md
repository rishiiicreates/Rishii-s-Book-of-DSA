---
type: concept
tags: [matrix, 2d-array, cpp, traversal]
date: 2026-07-01
---
# Matrix

## Problem Statement
Understand how to work with a 2D array (or matrix) to perform grid-based operations, traversals, and coordinate mapping.

---

## Approach: 2D Arrays and Memory Mapping

A **Matrix** is a two-dimensional array. In C++, this can be implemented as an array of arrays (e.g., `vector<vector<int>>`) or mathematically mapped onto a flattened 1D array.

For a matrix of dimensions $R \times C$ (rows $\times$ columns):
If implemented as a flat 1D array of size $R \times C$, the 2D coordinate $(i, j)$ maps to the 1D index:
$$ \text{Index} = i \times C + j $$

If implemented as a `vector<vector<int>>`, memory might not be perfectly contiguous across rows, but row access and column access are straightforward. Traversals require nested loops iterating over rows and columns.

### Row-Major vs Column-Major Order
C++ natively stores 2D arrays in **row-major order**, meaning contiguous elements in memory belong to the same row. Traversing row-by-row is cache-friendly and significantly faster than traversing column-by-column.

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
    int rows = 3, cols = 4;
    
    // Initialize an R x C matrix with all 0s
    vector<vector<int>> matrix(rows, vector<int>(cols, 0));
    
    // Row-major traversal (Cache friendly)
    for(int i = 0; i < rows; ++i) {
        for(int j = 0; j < cols; ++j) {
            // Assigning values based on 1D flat index mapping
            matrix[i][j] = i * cols + j;
        }
    }
    
    // Print the matrix
    for(int i = 0; i < rows; ++i) {
        for(int j = 0; j < cols; ++j) {
            cout << matrix[i][j] << " ";
        }
        cout << "\n";
    }
    
    return 0;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(R \times C)$ to traverse every element in the matrix.
- **Space Complexity:** $O(R \times C)$ to store the matrix.

> [!tip]
> For highly performance-critical code involving large matrices, use a flattened 1D `vector<int>` of size $R \times C$ and calculate indices manually. This guarantees cache locality and avoids the overhead of multiple vector allocations.
