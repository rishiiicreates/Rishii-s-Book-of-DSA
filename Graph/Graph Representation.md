# Graph Representation

## Problem Statement
- represent a graph using either an adjacency [[Matrix]] or an adjacency list to allow efficient traversal and querying.

## Approach / Intuition
- use an [[Adjacency Matrix]] for dense graphs where edge lookups are frequent. Use an [[Adjacency List]] for sparse graphs to save memory and quickly iterate over neighbors.

## Time & Space Complexity
- **[[time Complexity]]:** O(V + E) to build
- **[[space Complexity]]:** O(V + E) for list, O(V^2) for matrix

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
- `std::vector`

## Edge Cases
- empty graph, graph with self-loops or multiple edges.

NEXT: [[Index]]
