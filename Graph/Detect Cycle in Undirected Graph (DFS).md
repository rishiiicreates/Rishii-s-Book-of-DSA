---
type: concept
tags: [graph, dfs, cycle-detection, cpp, recursion]
date: 2026-07-01
---
# Detect Cycle in Undirected Graph (DFS)

## Problem Statement
Given an Undirected Geometric Graph mapping $V$ vertices and $E$ edges, mathematically evaluate the topology to ascertain the existence of at least one structurally closed cycle utilizing a Depth-First Search recursive descent architecture.

---

## Approach: Depth-First Search with Parent Tracking

This is the exact mathematical dual to the BFS approach, substituting the iterative Breadth-First mapping with absolute Recursive Depth-First propagation.
The fundamental topological rule identically holds: If a traversal algebraically evaluates a geometric vertex $N$ that is already structurally logged as `visited`, and $N$ does not mathematically equal the direct hierarchical `parent` of the current operational vertex, an explicit topological cycle is proven.

1. Construct the identical geometric global `visited` boundary array.
2. Plunge recursively: `dfs(node, parent, visited, adj)`.
3. For every topologically mapped neighbor of `node`:
   - If `!visited[neighbor]`, inductively recurse `dfs(neighbor, node)`. If it propagates `true`, short-circuit the execution.
   - If `visited[neighbor] && neighbor != parent`, a structural cross-edge exists. Immediately return `true`.

---

## Code Implementation

```cpp
#include <vector>

using namespace std;

bool detectCycleDFS(int node, int parent, vector<int>& vis, vector<int> adj[]) {
    vis[node] = 1; // Mark structural vertex as globally bound
    
    // Deep topological propagation
    for (auto adjacentNode : adj[node]) {
        if (!vis[adjacentNode]) {
            if (detectCycleDFS(adjacentNode, node, vis, adj)) {
                return true;
            }
        }
        // Vertex mathematically active but topologically detached from parent -> Cycle
        else if (adjacentNode != parent) {
            return true;
        }
    }
    
    return false;
}

bool isCycle(int V, vector<int> adj[]) {
    vector<int> vis(V, 0);
    
    // Isolate topologically disconnected graph sub-components
    for (int i = 0; i < V; i++) {
        if (!vis[i]) {
            if (detectCycleDFS(i, -1, vis, adj)) {
                return true;
            }
        }
    }
    
    return false;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(V + 2E)$ absolute limit. Geometrically identical to BFS; every vertex and its contiguous structural edges are recursively mapped.
- **Space Complexity:** $O(V)$ bounding the recursion execution stack depth in a degenerate path, plus $O(V)$ for the spatial boolean array.

> [!important]
> **Call Stack Vulnerability:** While DFS operates mathematically identically to BFS in temporal complexity, the recursive topology suffers from $O(V)$ sequential call stack memory allocation. A strictly skewed line graph of $10^5$ vertices will reliably trigger Stack Overflow violations on standard environments. BFS is algebraically safer for immense graphs.
