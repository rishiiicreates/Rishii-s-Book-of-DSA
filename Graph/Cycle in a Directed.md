---
type: concept
tags: [graph, cpp, cycle-detection]
date: 2026-06-30
---
# Cycle in a Directed Graph

## Problem Statement
Detect if a directed graph contains a cycle.

## Approach / Intuition
Maintain a visited array and a path visited array during [[DFS]]. If we reach a node that is already marked in the current path visited array, a cycle is detected.

## Time & Space Complexity
- **[[Time Complexity]]:** O(V + E)
- **[[Space Complexity]]:** O(V)

## Sample Code
```cpp
#include <vector>
using namespace std;

bool dfs(int node, vector<int> adj[], vector<bool>& vis, vector<bool>& pathVis) {
    vis[node] = true;
    pathVis[node] = true;
    for(auto it : adj[node]) {
        if(!vis[it]) {
            if(dfs(it, adj, vis, pathVis)) return true;
        } else if(pathVis[it]) {
            return true;
        }
    }
    pathVis[node] = false;
    return false;
}

bool isCyclic(int V, vector<int> adj[]) {
    vector<bool> vis(V, false);
    vector<bool> pathVis(V, false);
    for(int i = 0; i < V; i++) {
        if(!vis[i]) {
            if(dfs(i, adj, vis, pathVis)) return true;
        }
    }
    return false;
}
```

## New Keywords / STL Used
`std::vector`

## Edge Cases
Self-loops, disconnected components, DAG.
