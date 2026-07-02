---
type: concept
tags: [graph, cpp, dfs]
date: 2026-06-30
---
# Depth First Search (DFS)

## Problem Statement
Traverse a graph structure starting from a source node, exploring as far as possible along each branch before backtracking.

## Approach / Intuition
Use [[Recursion]] (implicit [[Stack]]) to explore nodes. Mark nodes as visited and recursively visit all unvisited neighbors.

## Time & Space Complexity
- **[[Time Complexity]]:** O(V + E)
- **[[Space Complexity]]:** O(V)

## Sample Code
```cpp
#include <vector>
using namespace std;

void dfs(int node, vector<int> adj[], vector<bool>& vis, vector<int>& ls) {
    vis[node] = true;
    ls.push_back(node);
    for(auto it : adj[node]) {
        if(!vis[it]) {
            dfs(it, adj, vis, ls);
        }
    }
}

vector<int> dfsOfGraph(int V, vector<int> adj[]) {
    vector<bool> vis(V, false);
    vector<int> ls;
    dfs(0, adj, vis, ls);
    return ls;
}
```

## New Keywords / STL Used
`std::vector`

## Edge Cases
Disconnected components, deep graphs causing stack overflow, cycles.
