---
type: concept
tags: [graph, cpp, cycle-detection]
date: 2026-06-30
---
# Cycle in an Undirected Graph

## Problem Statement
Detect if an undirected graph contains a cycle.

## Approach / Intuition
Use [[BFS]] or [[DFS]] while keeping track of the parent node. If we visit a node that is already visited and it is not the parent of the current node, a cycle exists.

## Time & Space Complexity
- **[[Time Complexity]]:** O(V + E)
- **[[Space Complexity]]:** O(V)

## Sample Code
```cpp
#include <vector>
using namespace std;

bool dfs(int node, int parent, vector<int> adj[], vector<bool>& vis) {
    vis[node] = true;
    for(auto it : adj[node]) {
        if(!vis[it]) {
            if(dfs(it, node, adj, vis)) return true;
        } else if(it != parent) {
            return true;
        }
    }
    return false;
}

bool isCycle(int V, vector<int> adj[]) {
    vector<bool> vis(V, false);
    for(int i = 0; i < V; i++) {
        if(!vis[i]) {
            if(dfs(i, -1, adj, vis)) return true;
        }
    }
    return false;
}
```

## New Keywords / STL Used
`std::vector`

## Edge Cases
Forest (multiple components), self-loops, parallel edges.
