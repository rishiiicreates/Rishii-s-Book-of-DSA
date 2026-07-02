---
type: concept
tags: [graph, dfs, cycle-detection, directed-graph, cpp]
date: 2026-06-30
---
# Detect Cycle in Directed Graph (DFS)

## Problem Statement
Given a **Directed Graph**, mathematically determine if it contains at least one **Cycle**.

*Definition:* A cycle in a directed graph implies there is a non-empty topological path that originates and terminates at the exact same vertex, following the strictly unidirectional edges.

---

## Approach: DFS Path State Tracking

Detecting a cycle in a Directed Graph differs profoundly from an Undirected Graph. In an undirected graph, encountering a visited node (that is not the immediate parent) mathematically guarantees a cycle. In a directed graph, encountering a visited node *does not* guarantee a cycle, because cross-edges can link separate branches without looping backward.

To isolate true cycles, we must mathematically differentiate between nodes that have been "fully processed" and nodes that are "currently active on the active recursive path". A cycle exists **if and only if** we encounter a node that is already present on our *current* DFS path.

1. **State Initialization:** Require two boolean arrays:
   - `visited[]`: Tracks if a node has ever been visited globally (prevents exponential redundancy).
   - `pathVisited[]` (or `inRecursionStack[]`): Tracks if a node is currently an active ancestor in the current, active recursive chain.
2. **Recursive Logic `dfs(node)`:**
   - Mark both `visited[node] = true` and `pathVisited[node] = true`.
   - Iterate over directed neighbors $v$.
   - If `!visited[v]`, recurse `dfs(v)`. If it returns true, propagate true.
   - If `visited[v] == true`, verify the cross-edge. If `pathVisited[v] == true`, we have definitively collided with our own recursive ancestor, establishing a directed cycle. Return `true`.
3. **Path Reversion:** Before the `dfs(node)` function yields control backward, it is mathematically exiting the path. We **must** reset `pathVisited[node] = false` to remove it from the active topological trace.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

bool checkCycleDFS(int node, vector<vector<int>>& adj, vector<bool>& visited, vector<bool>& pathVisited) {
    // 1. Lock node globally and structurally to current path
    visited[node] = true;
    pathVisited[node] = true;
    
    // 2. Directed Propagation
    for (int neighbor : adj[node]) {
        // Discovered uncharted vertex
        if (!visited[neighbor]) {
            if (checkCycleDFS(neighbor, adj, visited, pathVisited)) {
                return true;
            }
        } 
        // Discovered visited vertex; Check topological back-edge
        else if (pathVisited[neighbor]) {
            return true; // Unambiguous directed cycle confirmed
        }
    }
    
    // 3. Backtrack phase: node is no longer part of the active path hierarchy
    pathVisited[node] = false;
    return false;
}

bool isCyclic(int V, vector<vector<int>>& adj) {
    vector<bool> visited(V, false);
    vector<bool> pathVisited(V, false); // Active stack trace
    
    // Exhaustive component validation
    for (int i = 0; i < V; ++i) {
        if (!visited[i]) {
            if (checkCycleDFS(i, adj, visited, pathVisited)) {
                return true;
            }
        }
    }
    return false;
}
```

> [!important]
> For extreme performance optimization, the dual `visited` and `pathVisited` arrays can be compressed into a singular integer `state` array: `0 = Unvisited`, `1 = Visiting (Active Path)`, `2 = Visited (Fully Exhausted)`. This optimizes memory and halves cache misses.

---

## Complexity Analysis
- **Time Complexity:** $O(V + E)$. The recursive function operates precisely once per vertex, evaluating every outgoing directed edge.
- **Space Complexity:** $O(V)$ required to store the two boolean state arrays (or unified integer array) and the bounded DFS recursive stack.
