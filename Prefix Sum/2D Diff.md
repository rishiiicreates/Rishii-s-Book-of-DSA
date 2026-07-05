# 2D Difference Array

## Problem Statement
- given a 2D matrix initialized to zeros, process multiple queries of the form `(r1, c1, r2, c2, X)` to add value `X` to the submatrix explicitly defined by top-left `(r1, c1)` and bottom-right `(r2, c2)`.

## Approach / Intuition
- elevate the 1D difference logic strictly to 2D coordinates. Add `X` to the starting corner `(r1, c1)` and the post-diagonal corner `(r2+1, c2+1)`, while explicitly subtracting `X` from the intersecting corners `(r2+1, c1)` and `(r1, c2+1)`. Finally, reconstruct the matrix natively utilizing a sequential 2D [[Prefix Sum]] sweep to propagate values accurately.

## Time & Space Complexity
- **[[time Complexity]]:** O(R*C + Q)
- **[[space Complexity]]:** O(R*C)

## Sample Code
```cpp
vector<vector<int>> process2DQueries(int rows, int cols, vector<vector<int>>& queries) {
    vector<vector<int>> diff(rows + 2, vector<int>(cols + 2, 0));
    for (auto& q : queries) {
        int r1 = q[0], c1 = q[1], r2 = q[2], c2 = q[3], X = q[4];
        diff[r1][c1] += X;
        diff[r2 + 1][c1] -= X;
        diff[r1][c2 + 1] -= X;
        diff[r2 + 1][c2 + 1] += X;
    }
    vector<vector<int>> res(rows, vector<int>(cols, 0));
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            int current = diff[i][j];
            if (i > 0) current += res[i-1][j];
            if (j > 0) current += res[i][j-1];
            if (i > 0 && j > 0) current -= res[i-1][j-1];
            res[i][j] = current;
        }
    }
    return res;
}
```

## New Keywords / STL Used
- none

## Edge Cases
- queries completely overlapping each other, queries brushing the extreme outer [[Matrix]] boundaries.

NEXT: [[Index]]
