---
type: concept
tags: [graph, bipartite, bfs, cpp]
date: 2026-06-30
---
# Bipartite Graph (BFS)

## Mathematical Definition
A **Bipartite Graph** is a topological structure $G = (V, E)$ whose vertices can be rigorously partitioned into two mutually exclusive and collectively exhaustive sets $U$ and $V$, such that every edge $e \in E$ unconditionally connects a vertex in $U$ to a vertex in $V$. 

Equivalently, a graph is Bipartite if and only if it contains **zero odd-length cycles**. 
Practically, this translates to a Graph Coloring Problem: Can we color every node using exactly 2 colors such that no two adjacent nodes share the identical color?

---

## Approach: BFS Color Alternation

We can mathematically validate bipartiteness utilizing a 2-color BFS radial expansion.
Let the colors be defined as scalar states `0` and `1`. Uncolored nodes hold state `-1`.

1. **State Initialization:** Define an array `color` of size $|V|$ initialized universally to `-1`.
2. **Disconnected Component Iteration:** Because the graph may disjoint, we loop $0 \dots V-1$. If `color[i] == -1`, trigger `BFS(i)`.
3. **BFS Execution:**
   - Seed the source node with `color = 0`.
   - Enqueue the node.
   - While traversing adjacent neighbors $v$ of current node $u$:
     - **Uncolored:** If `color[v] == -1`, assign it the exact inverse color of its parent: `color[v] = 1 - color[u]`, and enqueue it.
     - **Colored (Validation):** If `color[v] != -1`, it implies a topological cycle has converged. Mathematically verify if `color[v] == color[u]`. If true, an odd-length cycle exists, destroying the bipartite constraint. Return `false` immediately.
4. If BFS fully exhausts all components without triggering the violation condition, the graph is unconditionally Bipartite. Return `true`.

---

## Code Implementation

```cpp
#include <vector>
#include <queue>
using namespace std;

bool checkBipartiteBFS(int start, vector<vector<int>>& adj, vector<int>& color) {
    queue<int> q;
    q.push(start);
    color[start] = 0; // Seeding initial subset mapping
    
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        
        for (int v : adj[u]) {
            if (color[v] == -1) {
                // Assign inverted topological state
                color[v] = 1 - color[u];
                q.push(v);
            } else if (color[v] == color[u]) {
                // Mathematical parity violation detected
                return false;
            }
        }
    }
    return true;
}

bool isBipartite(int V, vector<vector<int>>& adj) {
    vector<int> color(V, -1);
    
    // Exhaustive sweep for disconnected components
    for (int i = 0; i < V; ++i) {
        if (color[i] == -1) {
            if (!checkBipartiteBFS(i, adj, color)) {
                return false;
            }
        }
    }
    return true;
}
```

> [!important]
> A Graph entirely devoid of cycles (Trees/Forests), or containing exclusively **even-length cycles**, is mathematically guaranteed to be Bipartite. Only an odd-length cycle can force a topological color collision.

---

## Complexity Analysis
- **Time Complexity:** $O(V + E)$. The dual loops execute a standard BFS where every vertex is enqueued exactly once and every edge evaluated twice (undirected). 
- **Space Complexity:** $O(V)$ allocated strictly for the `color` state array and the maximum dynamic footprint of the queue.
