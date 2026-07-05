# Number of Islands

## Problem Statement
- given a 2D binary grid, count the number of islands. An island is surrounded by water (0s) and is formed by connecting adjacent lands (1s) horizontally or vertically.

## Approach / Intuition
- traverse the grid and whenever an unvisited land ('1') is found, increment the island count and trigger a [[BFS]] or [[DFS]] to mark the entire connected component as visited.

## Time & Space Complexity
- **[[time Complexity]]:** O(N * M)
- **[[space Complexity]]:** O(N * M)

## Sample Code
```cpp
#include <vector>
using namespace std;

void dfs(int r, int c, vector<vector<char>>& grid) {
    if(r < 0 || r >= grid.size() || c < 0 || c >= grid[0].size() || grid[r][c] == '0') return;
    grid[r][c] = '0';
    dfs(r - 1, c, grid);
    dfs(r + 1, c, grid);
    dfs(r, c - 1, grid);
    dfs(r, c + 1, grid);
}

int numIslands(vector<vector<char>>& grid) {
    int islands = 0;
    for(int i = 0; i < grid.size(); i++) {
        for(int j = 0; j < grid[0].size(); j++) {
            if(grid[i][j] == '1') {
                islands++;
                dfs(i, j, grid);
            }
        }
    }
    return islands;
}
```

## New Keywords / STL Used
- `std::vector`

## Edge Cases
- all 1s, all 0s, empty grid.

NEXT: [[Index]]
