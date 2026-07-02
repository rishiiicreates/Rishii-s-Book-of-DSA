---
type: concept
tags: [graph, bfs, matrix, cpp, state-machine]
date: 2026-07-01
---
# Rotting Oranges

## Problem Statement
Given an $M \times N$ spatial matrix grid, where each topological cell can assume three discrete states:
- `0`: Absolute Void (Empty Cell)
- `1`: Geometrically Isolated (Fresh Orange)
- `2`: Contagious Origin (Rotten Orange)

At every discrete temporal epoch ($T \to T+1$), any fresh orange that is 4-directionally adjacent to a rotten orange structurally decays to state `2`.
Compute the mathematical minimum temporal duration required to propagate decay to every contiguous fresh orange. If topological isolation renders complete decay impossible, return `-1`.

---

## Approach: Multi-Source Breadth-First Search (BFS)

Because decay structurally propagates outward identically and symmetrically from ALL origin sources simultaneously, a standard single-source traversal algebraically fails.
We must construct a **Multi-Source BFS** topology:
1. Initialize a generic `queue` mapped to state tuples: `{ {row, col}, temporal_epoch }`.
2. Iteratively map the spatial grid $O(M \times N)$. Inject EVERY coordinate mapping `grid[i][j] == 2` into the initial BFS frontier queue with $T = 0$. Concurrently, aggregate a scalar `fresh_count`.
3. Geometrically process the BFS queue. For the dequeued tuple, apply 4-directional matrix vector addition $(dr, dc) \in \{(-1,0), (1,0), (0,-1), (0,1)\}$.
4. If a geometric neighbor maps to a valid boundary AND holds state `1`:
   - Mutate its state matrix directly: `grid[nr][nc] = 2` (prevents redundant cyclic visitation).
   - Inject into queue with temporal mutation: $T + 1$.
   - Decrement `fresh_count`.
5. Upon queue exhaustion, mathematically evaluate isolation: If `fresh_count > 0`, return `-1`. Otherwise, return the absolute maximum $T$ observed.

---

## Code Implementation

```cpp
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

int orangesRotting(vector<vector<int>>& grid) {
    if (grid.empty()) return 0;
    
    int m = grid.size();
    int n = grid[0].size();
    
    // BFS State Geometry: {{row, col}, time}
    queue<pair<pair<int, int>, int>> q;
    
    int fresh_count = 0;
    
    // Aggregate initial Multi-Source Frontier
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 2) {
                q.push({{i, j}, 0});
            } else if (grid[i][j] == 1) {
                fresh_count++;
            }
        }
    }
    
    int max_time = 0;
    // 4-Directional spatial translation vectors
    int dr[] = {-1, 0, 1, 0};
    int dc[] = {0, 1, 0, -1};
    
    // BFS execution topological queue
    while (!q.empty()) {
        int r = q.front().first.first;
        int c = q.front().first.second;
        int t = q.front().second;
        
        max_time = max(max_time, t);
        q.pop();
        
        // Propagate decay geometry
        for (int i = 0; i < 4; i++) {
            int nr = r + dr[i];
            int nc = c + dc[i];
            
            // Spatial boundary validation and state verification
            if (nr >= 0 && nr < m && nc >= 0 && nc < n && grid[nr][nc] == 1) {
                grid[nr][nc] = 2; // Geometrically rot to prevent redundant queues
                q.push({{nr, nc}, t + 1});
                fresh_count--;
            }
        }
    }
    
    // Evaluate Topological Isolation
    return (fresh_count == 0) ? max_time : -1;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(M \times N)$ strict bound limit. The algorithm mathematically iterates the initial matrix to extract the frontier, and subsequently executes the BFS queue where each geometric cell is evaluated structurally at most exactly once.
- **Space Complexity:** $O(M \times N)$ bounding the absolute maximum geometry of the BFS queue (e.g., in a theoretical topology where every cell is entirely saturated with rotten oranges).

> [!important]
> **DFS Impossibility Theorem:** This topological problem mathematically forbids a Depth-First Search (DFS) resolution. DFS structurally evaluates contiguous paths until isolation exhaustion, inherently violating the absolute temporal symmetry required to model simultaneous wave propagation across disjoint topologies.
