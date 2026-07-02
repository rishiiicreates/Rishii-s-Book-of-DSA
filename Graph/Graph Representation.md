---
type: concept
tags: [graph, cpp, representation]
date: 2026-06-30
---
# Graph Representation

## Problem Statement
Represent a graph using either an adjacency matrix or an adjacency list to allow efficient traversal and querying.

## Approach / Intuition
Use an [[Adjacency Matrix]] for dense graphs where edge lookups are frequent. Use an [[Adjacency List]] for sparse graphs to save memory and quickly iterate over neighbors.

## Time & Space Complexity
- **[[Time Complexity]]:** O(V + E) to build
- **[[Space Complexity]]:** O(V + E) for list, O(V^2) for matrix

## Sample Code
```cpp
#include <vector>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> adj(n + 1);
    for(int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    return 0;
}
```

## New Keywords / STL Used
`std::vector`

## Edge Cases
Empty graph, graph with self-loops or multiple edges.
