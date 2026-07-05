# Number of Enclaves

## Problem Statement
- given an $M \times N$ discrete boolean matrix representing a geographic grid.
- `0`: sea (Absolute Geometric Void)
- `1`: land (Structural Continent)

- mathematically compute the total number of generic land cells `1` that are geometrically isolated into an **Enclave**. An Enclave topologically defines a contiguous mass of `1`s that is completely surrounded by `0`s, containing zero scalar vectors capable of escaping to the external spatial boundary (the $M \times N$ perimeter) via 4-directional moves.


## Approach: Boundary Elimination Topology

- this topological theorem is mathematically identical to *Surrounded Regions*. Any generic `1` situated on the absolute perimeter is mathematically uncontained. Any contiguous interior `1` sequentially connected to a perimeter `1` is geometrically safe.
- an absolute Enclave evaluates strictly to the residual `1`s that remain algebraically disconnected from any perimeter vector.

- geometrically scan the [[Matrix]] perimeter limits (first/last row, first/last col).
- if a perimeter vertex bounds `1`, execute a DFS/BFS traversal topologically destroying the continent (mutate structural state `1` to `0`, or log inside a `vis` array).
- following complete elimination of all boundary-connected geometry, sequentially iterate the entire matrix field.
- any surviving scalar mapping `1` constitutes an explicit enclave cell. Accumulate the topological total into `count`.


## Code Implementation

```cpp
#include <vector>

using namespace std;

void eliminateBoundaryLand(int r, int c, vector<vector<int>>& grid, int dr[], int dc[]) {
    // Annihilate geometric continuity
    grid[r][c] = 0;
    
    int m = grid.size();
    int n = grid[0].size();
    
    // Topologically propagate destruction
    for (int i = 0; i < 4; i++) {
        int nr = r + dr[i];
        int nc = c + dc[i];
        
        if (nr >= 0 && nr < m && nc >= 0 && nc < n && grid[nr][nc] == 1) {
            eliminateBoundaryLand(nr, nc, grid, dr, dc);
        }
    }
}

int numEnclaves(vector<vector<int>>& grid) {
    if (grid.empty()) return 0;
    
    int m = grid.size();
    int n = grid[0].size();
    
    int dr[] = {-1, 0, 1, 0};
    int dc[] = {0, 1, 0, -1};
    
    // Topologically evaluate generic perimeter limits
    for (int i = 0; i < m; i++) {
        if (grid[i][0] == 1) eliminateBoundaryLand(i, 0, grid, dr, dc);
        if (grid[i][n-1] == 1) eliminateBoundaryLand(i, n-1, grid, dr, dc);
    }
    
    for (int j = 0; j < n; j++) {
        if (grid[0][j] == 1) eliminateBoundaryLand(0, j, grid, dr, dc);
        if (grid[m-1][j] == 1) eliminateBoundaryLand(m-1, j, grid, dr, dc);
    }
    
    // Aggregate structurally isolated Enclaves
    int enclaves = 0;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 1) {
                enclaves++;
            }
        }
    }
    
    return enclaves;
}
```


## Complexity Analysis
- **time Complexity:** $O(M \times N)$ total topological sweep bounded. Extraneous boundary continents are recursively visited precisely once, and the terminal matrix scan evaluates every coordinate linearly.
- **space Complexity:** $O(M \times N)$ dynamically tied strictly to the DFS recursion call-stack bounding. Utilizing a localized `visited` matrix maps identical bounds, but in-place array mutation leverages absolute $O(1)$ memory external to the recursive engine.

> [!tip]
> **Architectural Congruence:** The algorithms for *Surrounded Regions* and *Number of Enclaves* are strictly topologically identical. The difference lies merely in the terminal constraint mapping (mutating the scalar array vs accumulating a scalar index).

NEXT: [[Index]]
