---
type: concept
tags: [graph, bfs, cycle-detection, cpp]
date: 2026-07-01
---
# Detect Cycle in Undirected Graph (BFS)

## Problem Statement
Given an Undirected Geometric Graph mapping $V$ vertices and $E$ edges, mathematically evaluate the topology to ascertain the existence of at least one structurally closed cycle. A cycle exists if a topological traversal path encounters a vertex that has already been spatially visited, and that vertex is algebraically NOT the immediate parent (predecessor) of the current vertex in the traversal tree.

---

## Approach: Breadth-First Search with Parent Tracking

To structurally isolate a cycle, we execute a localized component BFS.
Because the graph is undirected, traversal inherently flows backward across the identical topological edge. We must mathematically prevent this false-positive cycle by mapping State Tuples: `{current_vertex, parent_vertex}`.

1. Geometrically construct a global `visited` boolean array. Iterate absolute $V$ dimensions to trigger isolated disconnected components.
2. For an unvisited component, instantiate a `queue` and push the structural seed `{start_vertex, -1}` mapping a generic null parent.
3. Iteratively execute the BFS topology:
   - For every connected topological neighbor:
     - If algebraically unvisited, mark it `true` and inject `{neighbor, current}` into the queue.
     - If algebraically visited AND `neighbor != parent`, we have mathematically discovered a disjoint structural collision. The topology contains a cycle.

---

## Code Implementation

```cpp
#include <vector>
#include <queue>

using namespace std;

bool detectCycleBFS(int src, vector<int> adj[], vector<int>& vis) {
    vis[src] = 1;
    // Spatial State Tracker: {vertex, parent_vertex}
    queue<pair<int, int>> q;
    q.push({src, -1});
    
    while (!q.empty()) {
        int node = q.front().first;
        int parent = q.front().second;
        q.pop();
        
        // Propagate geometric boundaries
        for (auto adjacentNode : adj[node]) {
            if (!vis[adjacentNode]) {
                vis[adjacentNode] = 1;
                q.push({adjacentNode, node});
            }
            // Disconnected topological visitation mapping -> Absolute Cycle
            else if (parent != adjacentNode) {
                return true;
            }
        }
    }
    return false;
}

bool isCycle(int V, vector<int> adj[]) {
    // Global connectivity isolation mapping
    vector<int> vis(V, 0);
    for (int i = 0; i < V; i++) {
        if (!vis[i]) {
            if (detectCycleBFS(i, adj, vis)) return true;
        }
    }
    return false;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(V + 2E)$ absolute limit. The outer loop mathematically evaluates all vertices. The internal BFS spans every topological edge. In an undirected graph, total edge traversal structurally evaluates to $2E$.
- **Space Complexity:** $O(V)$ allocated to the absolute spatial bounds of the `visited` array and the BFS queue constraints.

> [!tip]
> **Component Saturation:** The absolute outer `for` loop mapping $1 \to V$ is structurally mandatory. A generically defined graph is not mathematically guaranteed to be a single globally connected component. Cycle detection must evaluate all isolated sub-graphs.
