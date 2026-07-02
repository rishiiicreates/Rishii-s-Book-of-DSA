---
type: concept
tags: [graph, matrix, connected-components, bfs, dfs, cpp]
date: 2026-06-30
---
# Number of Islands

## Problem Statement
Given an $M \times N$ 2D binary topological grid `grid` representing a conceptual map of `'1'`s (land) and `'0'`s (water), mathematically calculate and return the total **Number of Islands**.

An island is defined as a contiguous maximal component of `'1'`s connected mathematically horizontally or vertically (4-directional adjacency). Assume all four boundaries defining the edge of the grid are infinitely surrounded by water.

---

## Approach: Spatial Connected Components (Matrix Graph)

This is a topological equivalent to computing [[Number of Provinces]], but mapped explicitly onto a Cartesian grid. Every cell $(r, c)$ containing a `'1'` functions as a graph vertex. Edges implicitly exist between contiguous horizontal/vertical `'1'`s.

The methodology identically dictates sweeping the matrix and triggering a [[Breadth First Search]] (or DFS) component exhaustion upon discovering any unvisited landmass.

1. **State Preservation:** We can maintain an $M \times N$ `visited` boolean matrix. 
   *(Optimization: We can mutate the input `grid` in-place, overwriting `'1'`s to `'0'`s as we traverse them, mathematically achieving $O(1)$ auxiliary space and eliminating the `visited` array entirely).*
2. **Matrix Sweep:**
   - Iterate over rows $r$ from $0 \dots M-1$ and columns $c$ from $0 \dots N-1$.
   - If `grid[r][c] == '1'`:
     - Increment `islands++`.
     - Execute `BFS(r, c)` (or `DFS(r, c)`) to sink the entire contiguous landmass by flipping all connected `'1'`s to `'0'`s.
3. **Traversal Constraints:** During BFS/DFS, rigorously validate that neighboring vectors $(nr, nc)$ remain strictly within the mathematical boundaries $0 \le nr < M$ and $0 \le nc < N$, and exclusively target `'1'` cells.

---

## Code Implementation (DFS - In-Place Mutation)

```cpp
#include <vector>
using namespace std;

// Directional Cartesian vectors (Up, Right, Down, Left)
const int dr[4] = {-1, 0, 1, 0};
const int dc[4] = {0, 1, 0, -1};

void sinkIsland(vector<vector<char>>& grid, int r, int c, int m, int n) {
    // 1. Sink the current vertex to prevent cyclic redundancy
    grid[r][c] = '0';
    
    // 2. Propagate radially
    for (int i = 0; i < 4; ++i) {
        int nr = r + dr[i];
        int nc = c + dc[i];
        
        // Mathematical boundary constraints and structural targeting
        if (nr >= 0 && nr < m && nc >= 0 && nc < n && grid[nr][nc] == '1') {
            sinkIsland(grid, nr, nc, m, n);
        }
    }
}

int numIslands(vector<vector<char>>& grid) {
    if (grid.empty() || grid[0].empty()) return 0;
    
    int m = grid.size();
    int n = grid[0].size();
    int islands = 0;
    
    // Exhaustive Cartesian sweep
    for (int r = 0; r < m; ++r) {
        for (int c = 0; c < n; ++c) {
            if (grid[r][c] == '1') {
                islands++;
                sinkIsland(grid, r, c, m, n); // Eradicate the component
            }
        }
    }
    
    return islands;
}
```

> [!important]
> Some variations of this problem permit **8-directional** connectivity (diagonals). If so, simply expand the coordinate delta vectors to accommodate all 8 topological neighbors: `dr[8] = {-1,-1,-1,0,0,1,1,1}`.

---

## Complexity Analysis
- **Time Complexity:** $O(M \times N)$. The dual `for` loops enforce an exhaustive scan of the grid. Inside the DFS execution, every `'1'` is strictly flipped to a `'0'` upon its first visitation. Consequently, no cell is processed by the DFS recursion more than once. The total theoretical overhead maps to the exact area of the grid.
- **Space Complexity:** $O(M \times N)$ exclusively bounded by the recursive call stack. In a pathological grid entirely composed of a singular zig-zag landmass, the depth of the DFS chain equals the total cellular volume. Implementing an explicit queue-based BFS drops this space constraint to $O(\min(M, N))$, optimizing spatial volatility.
