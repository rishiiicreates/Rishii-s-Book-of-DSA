# Connected Components in an Undirected Graph

## Problem Statement
- count the number of connected components in an undirected graph given `V` vertices and `E` edges.

## Approach / Intuition
- iterate through all nodes. For every unvisited node, increment the component count and run a [[DFS]] or [[BFS]] to visit all nodes in its component.

## Time & Space Complexity
- **[[time Complexity]]:** O(V + E)
- **[[space Complexity]]:** O(V + E)

## Sample Code
```cpp
#include <vector>
using namespace std;

void dfs(int node, vector<int> adj[], vector<bool>& vis) {
    vis[node] = true;
    for(auto it : adj[node]) {
        if(!vis[it]) {
            dfs(it, adj, vis);
        }
    }
}

int countComponents(int V, vector<int> adj[]) {
    vector<bool> vis(V, false);
    int count = 0;
    for(int i = 0; i < V; i++) {
        if(!vis[i]) {
            count++;
            dfs(i, adj, vis);
        }
    }
    return count;
}
```

## New Keywords / STL Used
- `std::vector`

## Edge Cases
- fully disconnected graph, complete graph, single node.

NEXT: [[Index]]
