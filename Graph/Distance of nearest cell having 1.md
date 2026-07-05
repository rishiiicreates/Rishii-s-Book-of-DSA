# Distance of Nearest Cell Having 1 (0/1 Matrix)

## Problem Statement
- given an $M \times N$ topological matrix comprised of discrete binary scalars `0` and `1`. Mathematically compute a structurally identical output matrix where every cell algebraically represents the minimum spatial Manhattan distance to the absolute nearest `1`.
- the Manhattan distance mapping between coordinates $(r_1, c_1)$ and $(r_2, c_2)$ evaluates strictly to $|r_1 - r_2| + |c_1 - c_2|$.


## Approach: Multi-Source Geometric BFS

- this topology maps exactly to a **Multi-Source BFS** propagation outward from all `1`s simultaneously.
- a naive single-source topology executes BFS identically from every `0`, violating efficiency bounds to $O((M \times N)^2)$.
- by injecting every `1` coordinate into an active frontier with distance `0`, the algorithm guarantees that any topological unvisited cell inherently evaluates its minimum geometric distance identically when it is first intersected by the outward expanding perimeter.

- construct a discrete geometric tracker `dist` matrix initialized to `0` and a boolean `visited` matrix.
- enqueue ALL spatial coordinates bounding `grid[i][j] == 1` mapping a state tuple `{ {r, c}, 0 }`. Execute `visited[i][j] = 1`.
- geometrically iterate the BFS queue. Pop $\{ {r, c}, d \}$, execute boundary propagation algebraically: $dr, dc \in \{(-1,0), (1,0), (0,-1), (0,1)\}$.
- if a neighbor bounds safely within the matrix constraints and evaluates `!visited`:
   - mutate its topological boundaries: `visited[nr][nc] = 1`.
   - mutate geometric vector map: `dist[nr][nc] = d + 1`.
   - inject mutated neighbor back to queue `{ {nr, nc}, d + 1 }`.


## Code Implementation

```cpp
#include <vector>
#include <queue>

using namespace std;

vector<vector<int>> nearest(vector<vector<int>> grid) {
    int m = grid.size();
    int n = grid[0].size();
    
    vector<vector<int>> vis(m, vector<int>(n, 0));
    vector<vector<int>> dist(m, vector<int>(n, 0));
    
    // Topologically map state: {{row, col}, distance}
    queue<pair<pair<int, int>, int>> q;
    
    // Inject Multi-Source Frontier Geometry
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 1) {
                q.push({{i, j}, 0});
                vis[i][j] = 1;
            }
        }
    }
    
    int dr[] = {-1, 0, 1, 0};
    int dc[] = {0, 1, 0, -1};
    
    while (!q.empty()) {
        int r = q.front().first.first;
        int c = q.front().first.second;
        int steps = q.front().second;
        q.pop();
        
        dist[r][c] = steps;
        
        for (int i = 0; i < 4; i++) {
            int nr = r + dr[i];
            int nc = c + dc[i];
            
            // Spatial boundary compliance & geometric uniqueness validation
            if (nr >= 0 && nr < m && nc >= 0 && nc < n && !vis[nr][nc]) {
                vis[nr][nc] = 1;
                q.push({{nr, nc}, steps + 1});
            }
        }
    }
    
    return dist;
}
```


## Complexity Analysis
- **time Complexity:** $O(M \times N)$ spatial iteration. The grid evaluates perfectly linearly; the Multi-Source algorithm structurally limits every cell intersection to $O(1)$.
- **space Complexity:** $O(M \times N)$ tracking the geometrical bounds for the BFS queue, `vis` array, and output `dist` matrix.

> [!tip]
> **Dynamic Programming Isomorphism:** This topology mathematically solves in pure $O(M \times N)$ time bounded to $O(1)$ extra space by executing a dual-pass Dynamic Programming algorithm. Pass 1: Propagate Top-Left scalar bounds. Pass 2: Propagate Bottom-Right scalar bounds. However, BFS guarantees a generic, structurally flexible architecture adaptable to graph obstacles.

NEXT: [[Index]]
