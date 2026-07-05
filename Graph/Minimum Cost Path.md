# Minimum Cost Path

## Problem Statement
- find the minimum cost to reach the bottom-right cell from the top-left cell in a grid, moving in 4 directions, where cell values represent the traversal cost.

## Approach / Intuition
- view the grid as a graph where each cell is a vertex and edges exist to 4-directional neighbors. Use [[Dijkstra]] algorithm with a min-heap to find the minimum cost path.

## Time & Space Complexity
- **[[time Complexity]]:** O(N^2 \log(N^2))
- **[[space Complexity]]:** O(N^2)

## Sample Code
```cpp
int minimumCostPath(vector<vector<int>>& grid) {
    int n = grid.size();
    vector<vector<int>> dist(n, vector<int>(n, 1e9));
    priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, greater<pair<int, pair<int, int>>>> pq;
    dist[0][0] = grid[0][0];
    pq.push({grid[0][0], {0, 0}});
    int dx[] = {-1, 0, 1, 0};
    int dy[] = {0, 1, 0, -1};
    while(!pq.empty()) {
        int cost = pq.top().first;
        int r = pq.top().second.first;
        int c = pq.top().second.second;
        pq.pop();
        if(r == n - 1 && c == n - 1) return cost;
        for(int i = 0; i < 4; i++) {
            int nr = r + dx[i];
            int nc = c + dy[i];
            if(nr >= 0 && nr < n && nc >= 0 && nc < n && cost + grid[nr][nc] < dist[nr][nc]) {
                dist[nr][nc] = cost + grid[nr][nc];
                pq.push({dist[nr][nc], {nr, nc}});
            }
        }
    }
    return -1;
}
```

## New Keywords / STL Used
- `priority_queue`, nested `pair`

## Edge Cases
- 1x1 grid, grid with uniform costs, path requiring zig-zag traversal.

NEXT: [[Index]]
