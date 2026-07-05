# Total Spanning Trees

## Problem Statement
- find the total number of spanning trees in a connected undirected graph.

## Approach / Intuition
- apply [[Kirchhoff's [[Matrix]] Tree Theorem]]. Construct the Laplacian matrix by subtracting the Adjacency matrix from the Degree matrix, remove any one row and one column, and calculate the absolute value of the determinant.

## Time & Space Complexity
- **[[time Complexity]]:** O(V^3)
- **[[space Complexity]]:** O(V^2)

## Sample Code
```cpp
int getDeterminant(vector<vector<int>>& mat, int n) {
    long long det = 1;
    for (int i = 0; i < n; i++) {
        int pivot = i;
        for (int j = i + 1; j < n; j++) {
            if (abs(mat[j][i]) > abs(mat[pivot][i])) {
                pivot = j;
            }
        }
        if (pivot != i) {
            swap(mat[i], mat[pivot]);
            det *= -1;
        }
        if (mat[i][i] == 0) return 0;
        for (int j = i + 1; j < n; j++) {
            int factor = mat[j][i] / mat[i][i];
            for (int k = i; k < n; k++) {
                mat[j][k] -= factor * mat[i][k];
            }
        }
        det *= mat[i][i];
    }
    return abs(det);
}
int spanningTrees(int n, vector<vector<int>>& edges) {
    vector<vector<int>> laplacian(n, vector<int>(n, 0));
    for (auto& e : edges) {
        int u = e[0], v = e[1];
        laplacian[u][u]++;
        laplacian[v][v]++;
        laplacian[u][v]--;
        laplacian[v][u]--;
    }
    vector<vector<int>> subMatrix(n - 1, vector<int>(n - 1));
    for (int i = 1; i < n; i++) {
        for (int j = 1; j < n; j++) {
            subMatrix[i - 1][j - 1] = laplacian[i][j];
        }
    }
    return getDeterminant(subMatrix, n - 1);
}
```

## New Keywords / STL Used
- `abs`, `swap`, Gaussian elimination

## Edge Cases
- disconnected graph (returns 0), completely connected graph, tree structure (returns 1).

NEXT: [[Index]]
