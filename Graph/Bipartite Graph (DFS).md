# Bipartite Graph (DFS)

## Problem Statement
- determine mathematically if a given graph is a **Bipartite Graph** utilizing a Recursive [[Depth First Search]] approach instead of BFS.

- *(definition recap: A graph is Bipartite if its vertices can be partitioned into two distinct sets such that no edge connects vertices within the same set, functionally equivalent to 2-coloring the graph without collisions).*


## Approach: Recursive Alternating Coloring

- the DFS logic is topologically symmetric to the BFS logic, transitioning from an explicit queue to the implicit recursive call stack.

- **state Preservation:** Maintain the exact same `color` state array of size $|V|$, initialized universally to `-1`.
- **recursive Descent `dfs(node, col, color, adj)`:**
   - immediately bind `color[node] = col`.
   - iterate across all mathematically adjacent neighbors $v$.
   - **uncolored Phase:** If `color[v] == -1`, recursively trigger `dfs(v, 1 - col, color, adj)`. If the recursive call bubbles up a `false` evaluation, propagate the `false` immediately to abort.
   - **collision Phase:** If `color[v] != -1` (meaning a cyclic path has converged back onto this node), rigidly evaluate if `color[v] == col`. If true, an odd-length cycle is confirmed. Return `false`.
- return `true` if the entire branch successfully terminates.


## Code Implementation

```cpp
#include <vector>
using namespace std;

// Recursive topological descent
bool checkBipartiteDFS(int u, int col, vector<vector<int>>& adj, vector<int>& color) {
    // 1. Lock mathematical state
    color[u] = col;
    
    // 2. Propagate to adjacent nodes
    for (int v : adj[u]) {
        // Node is completely undiscovered
        if (color[v] == -1) {
            // Recurse with inverted parity (1 - col)
            if (!checkBipartiteDFS(v, 1 - col, adj, color)) {
                return false; // Propagate structural failure
            }
        } 
        // Node is discovered; check parity collision
        else if (color[v] == col) {
            return false;
        }
    }
    
    return true; // Topology holds
}

bool isBipartite(int V, vector<vector<int>>& adj) {
    vector<int> color(V, -1);
    
    // Global iterator guarantees validation of fully disjoint components
    for (int i = 0; i < V; ++i) {
        if (color[i] == -1) {
            // Seed initial parity arbitrarily as 0
            if (!checkBipartiteDFS(i, 0, adj, color)) {
                return false; 
            }
        }
    }
    return true;
}
```

> [!tip]
> Between DFS and BFS for Bipartite checking, there is negligible asymptotic difference. However, DFS recursion depth can hit $O(V)$, risking stack overflow on aggressively deep topological branches (e.g., a $10^5$ node linked list). For pure memory stability in production environments, BFS is frequently preferred.


## Complexity Analysis
- **time Complexity:** $O(V + E)$. The recursive function `checkBipartiteDFS` is executed exactly once per vertex. Edges are scanned $O(E)$ times.
- **space Complexity:** $O(V)$. The `color` array consumes strict $O(V)$ space. The recursive execution stack requires auxiliary space proportional to the maximum topological branch depth, bounding at $O(V)$.

NEXT: [[Index]]
