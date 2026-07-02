---
type: concept
tags: [matrix, array, cpp, rotation, transformation]
date: 2026-06-30
---
# Rotate a Matrix by 90 Degrees

## Problem Statement
Given an $N \times N$ 2D matrix representing an image, rotate the image by **90 degrees clockwise** in-place.
You must modify the 2D matrix directly. Do not allocate another 2D matrix to perform the rotation.

---

## Approach: Mathematical Orthogonal Transposition

Attempting to track the cyclic permutations of 4 individual elements moving in a square ring can be extremely error-prone.
Instead, we rely on a fundamental property of linear algebra: a 90-degree clockwise rotation matrix is equivalent to a transpose matrix followed by a horizontal reflection.

1. **Transpose the Matrix:** The transpose operation swaps elements across the main diagonal: $M[i][j] \leftrightarrow M[j][i]$. This converts all rows into columns.
2. **Reverse Each Row:** Reversing every row horizontally completes the visual 90-degree clockwise shift.

*Bonus Intuition:* If the problem asks for a 90-degree **counter-clockwise** rotation, simply transpose the matrix and then reverse each **column** vertically!

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    
    // Step 1: Transpose the matrix
    for (int i = 0; i < n; i++) {
        // Start j from i to only process the upper triangle
        for (int j = i; j < n; j++) {
            swap(matrix[i][j], matrix[j][i]);
        }
    }
    
    // Step 2: Reverse each row horizontally
    for (int i = 0; i < n; i++) {
        reverse(matrix[i].begin(), matrix[i].end());
    }
}
```

> [!warning]
> During the transpose step, the inner loop must strictly initialize at `j = i`. If you initialize at `j = 0`, you will swap the elements twice across the diagonal, effectively completely reverting the matrix back to its original state!

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N^2)$. The transpose step performs $\approx \frac{N^2}{2}$ operations. The reverse step performs $\approx \frac{N^2}{2}$ operations. Both scale linearly with respect to the total number of cells.
- **Space Complexity:** $\mathcal{O}(1)$. All coordinate geometry shifts are done utilizing the standard library swapping mechanisms in-place.
