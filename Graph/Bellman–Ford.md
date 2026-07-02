---
type: concept
tags: [graph, cpp, shortest-path, bellman-ford]
date: 2026-06-30
---
# Bellman-Ford

## Problem Statement
Find the shortest path from a single source to all vertices in a directed graph, even with negative edge weights, and detect negative weight cycles.

## Approach / Intuition
Relax all edges exactly V-1 times. If any edge can still be relaxed on the Vth iteration, a [[Negative Weight Cycle]] exists.

## Time & Space Complexity
- **[[Time Complexity]]:** O(V \times E)
- **[[Space Complexity]]:** O(V)

## Sample Code
```cpp
vector<int> bellman_ford(int V, vector<vector<int>>& edges, int S) {
    vector<int> dist(V, 1e8);
    dist[S] = 0;
    for (int i = 0; i < V - 1; i++) {
        for (auto it : edges) {
            int u = it[0], v = it[1], wt = it[2];
            if (dist[u] != 1e8 && dist[u] + wt < dist[v]) {
                dist[v] = dist[u] + wt;
            }
        }
    }
    for (auto it : edges) {
        int u = it[0], v = it[1], wt = it[2];
        if (dist[u] != 1e8 && dist[u] + wt < dist[v]) {
            return {-1};
        }
    }
    return dist;
}
```

## New Keywords / STL Used
`vector`

## Edge Cases
Graphs containing a negative cycle, graphs with negative weights but no negative cycle.
