# Rotten Oranges

## Problem Statement
- given a grid where 0 is empty, 1 is a fresh orange, and 2 is a rotten orange, find the minimum time required to rot all oranges. A rotten orange rots adjacent fresh oranges in 1 unit of time.

## Approach / Intuition
- push all initially rotten oranges into a [[Queue]] with time 0. Use multi-source [[BFS]] to rot adjacent fresh oranges. Keep track of the maximum time and remaining fresh oranges.

## Time & Space Complexity
- **[[time Complexity]]:** O(N * M)
- **[[space Complexity]]:** O(N * M)

## Sample Code
```cpp
#include <vector>
#include <queue>
using namespace std;

int orangesRotting(vector<vector<int>>& grid) {
    int n = grid.size(), m = grid[0].size();
    queue<pair<pair<int, int>, int>> q;
    int fresh = 0;
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < m; j++) {
            if(grid[i][j] == 2) q.push({{i, j}, 0});
            if(grid[i][j] == 1) fresh++;
        }
    }
    int tm = 0;
    int drow[] = {-1, 0, 1, 0};
    int dcol[] = {0, 1, 0, -1};
    while(!q.empty()) {
        int r = q.front().first.first;
        int c = q.front().first.second;
        int t = q.front().second;
        tm = max(tm, t);
        q.pop();
        for(int i = 0; i < 4; i++) {
            int nrow = r + drow[i];
            int ncol = c + dcol[i];
            if(nrow >= 0 && nrow < n && ncol >= 0 && ncol < m && grid[nrow][ncol] == 1) {
                grid[nrow][ncol] = 2;
                q.push({{nrow, ncol}, t + 1});
                fresh--;
            }
        }
    }
    if(fresh > 0) return -1;
    return tm;
}
```

## New Keywords / STL Used
- `std::queue`, `std::pair`

## Edge Cases
- no fresh oranges initially, unreachable fresh oranges, no rotten oranges initially.

NEXT: [[Index]]
