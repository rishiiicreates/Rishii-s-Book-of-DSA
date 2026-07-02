---
type: concept
tags: [graph, cpp, multi-source-bfs, grid]
date: 2026-06-30
---
# Distance of nearest 1

## Problem Statement
Given a binary grid, find the distance of the nearest 1 for every 0.

## Approach / Intuition
Use multi-source [[BFS]]. Enqueue all cells containing 1 initially with a distance of 0. Explore level by level to update the distances of all adjacent 0s.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N \times M)
- **[[Space Complexity]]:** O(N \times M)

## Sample Code
```cpp
vector<vector<int>> nearest(vector<vector<int>>& grid) {
    int n = grid.size();
    int m = grid[0].size();
    vector<vector<int>> vis(n, vector<int>(m, 0));
    vector<vector<int>> dist(n, vector<int>(m, 0));
    queue<pair<pair<int, int>, int>> q;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) {
                q.push({{i, j}, 0});
                vis[i][j] = 1;
            }
        }
    }
    int dx[] = {-1, 0, 1, 0};
    int dy[] = {0, 1, 0, -1};
    while (!q.empty()) {
        int r = q.front().first.first;
        int c = q.front().first.second;
        int d = q.front().second;
        q.pop();
        dist[r][c] = d;
        for (int i = 0; i < 4; i++) {
            int nr = r + dx[i];
            int nc = c + dy[i];
            if (nr >= 0 && nr < n && nc >= 0 && nc < m && !vis[nr][nc]) {
                vis[nr][nc] = 1;
                q.push({{nr, nc}, d + 1});
            }
        }
    }
    return dist;
}
```

## New Keywords / STL Used
`queue`, nested `pair`

## Edge Cases
Grid with all 1s, grid with all 0s, 1x1 grid.
