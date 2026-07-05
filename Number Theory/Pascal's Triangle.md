# Pascal's Triangle

## Problem Statement
- generate the first $N$ rows of Pascal's triangle.

## Approach / Intuition
- use [[Dynamic Programming]] to build the triangle row by row. Each element is the sum of the two elements directly above it from the previous row: `triangle[i][j] = triangle[i-1][j-1] + triangle[i-1][j]`.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N^2)$
- **[[space Complexity]]:** $O(N^2)$ to store all rows, $O(N)$ if only the last row is needed.

## Sample Code
```cpp
vector<vector<int>> generatePascal(int numRows) {
    vector<vector<int>> triangle;
    for (int i = 0; i < numRows; ++i) {
        vector<int> row(i + 1, 1);
        for (int j = 1; j < i; ++j) {
            row[j] = triangle[i - 1][j - 1] + triangle[i - 1][j];
        }
        triangle.push_back(row);
    }
    return triangle;
}
```

## New Keywords / STL Used
- `vector<vector>`

## Edge Cases
- $n = 0$, $N = 1$.

NEXT: [[Index]]
