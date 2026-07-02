---
type: concept
tags: [graph, traversal, bfs, queue, cpp]
date: 2026-06-30
---
# Breadth First Search (BFS) in Graph

## Problem Statement
Given a Graph $G = (V, E)$ and an initial source vertex $S$, mathematically traverse the graph such that all vertices at topological distance $d$ from $S$ are fully exhausted before any vertex at distance $d+1$ is explored.

---

## Approach: Queue-Based Level Traversal

Unlike trees, generic graphs contain cycles. Traversing a cyclic topology without a strict memory state guarantees an infinite topological loop. Therefore, we rigidly maintain an absolute $O(V)$ state array, typically denoted as `visited`.

1. **State Initialization:** Instantiate a dynamic Queue data structure and a boolean array `visited` of size $|V|$, initially all `false`.
2. **Source Seeding:** Enqueue the origin vertex $S$ and mark `visited[S] = true`.
3. **Radial Expansion:**
   - While the queue is non-empty, dequeue a vertex $U$.
   - Iterate mathematically through all adjacent neighbors $V$ of $U$.
   - If a neighbor $V$ has NOT been visited (`!visited[V]`):
     - Unconditionally mark `visited[V] = true` **immediately**.
     - Enqueue $V$.

> [!important]
> Marking a node as visited *when it is enqueued* is mathematically critical. If you defer marking it until it is *dequeued*, multiple paths reaching the same unvisited node will redundantly enqueue it, destroying the topological boundary and causing exponential complexity explosion.

---

## Code Implementation

```cpp
#include <vector>
#include <queue>
#include <iostream>
using namespace std;

void bfs(int V, vector<vector<int>>& adj, int startNode) {
    // Structural state preservation to avoid cyclic traps
    vector<bool> visited(V, false);
    queue<int> q;
    vector<int> bfs_traversal; // Store result
    
    // Seed initial state
    q.push(startNode);
    visited[startNode] = true; 
    
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        
        bfs_traversal.push_back(u);
        
        // Exhaustive lateral expansion
        for (int v : adj[u]) {
            if (!visited[v]) {
                visited[v] = true; // Lock topological node immediately
                q.push(v);
            }
        }
    }
}
```

> [!warning]
> If the graph is mathematically **disconnected**, executing BFS from a single $S$ will only traverse the contiguous component containing $S$. To traverse the absolute entirety of a disconnected graph, you must wrap the BFS call in an external $O(V)$ iterator over the `visited` array, seeding a new BFS for every unvisited origin.

---

## Complexity Analysis
- **Time Complexity:** $O(V + E)$. In the absolute worst case, every single vertex is enqueued exactly once ($O(V)$), and every single edge is traversed exactly once (in directed graphs) or twice (in undirected graphs) ($O(E)$). Thus, the mathematical bound is strictly linear relative to the structural volume.
- **Space Complexity:** $O(V)$. The `visited` array consumes $O(V)$ bits/bytes. The Queue dynamically expands up to a theoretical max size of $O(V)$ (e.g., a massive star topology).
