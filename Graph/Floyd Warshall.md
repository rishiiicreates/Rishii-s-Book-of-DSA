---
type: concept
tags: [graph, cpp, shortest-path, floyd-warshall]
date: 2026-06-30
---
# Floyd Warshall

## Problem Statement
Find the shortest paths between all pairs of vertices in a weighted directed graph, handling negative edges but no negative cycles.

## Approach / Intuition
Use [[Dynamic Programming]] to iteratively update the shortest path between every pair of nodes considering every node as an intermediate point.

## Time & Space Complexity
- **[[Time Complexity]]:** O(V^3)
- **[[Space Complexity]]:** O(V^2)

## Sample Code
```cpp
void shortest_distance(vector<vector<int>>& matrix) {
    int n = matrix.size();
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (matrix[i][j] == -1) matrix[i][j] = 1e9;
            if (i == j) matrix[i][j] = 0;
        }
    }
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if(matrix[i][k] != 1e9 && matrix[k][j] != 1e9)
                    matrix[i][j] = min(matrix[i][j], matrix[i][k] + matrix[k][j]);
            }
        }
    }
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (matrix[i][j] == 1e9) matrix[i][j] = -1;
        }
    }
}
```

## New Keywords / STL Used
`vector`, `min`

## Edge Cases
Graph with negative weights, isolated nodes, absent edges marked as -1.
