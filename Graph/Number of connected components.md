# Number of connected components

## Problem Statement
- count the number of disconnected components in an undirected graph.

## Approach / Intuition
- iterate over all vertices and perform [[DFS]] or [[BFS]] for any unvisited node. Each complete traversal of an unvisited node marks one entire component.

## Time & Space Complexity
- **[[time Complexity]]:** O(V + E)
- **[[space Complexity]]:** O(V)

## Sample Code
```cpp
void dfs(int node, vector<int> adj[], vector<int>& vis) {
    vis[node] = 1;
    for (auto it : adj[node]) {
        if (!vis[it]) {
            dfs(it, adj, vis);
        }
    }
}
int countComponents(int V, vector<vector<int>>& edges) {
    vector<int> adj[V];
    for(auto it : edges) {
        adj[it[0]].push_back(it[1]);
        adj[it[1]].push_back(it[0]);
    }
    vector<int> vis(V, 0);
    int count = 0;
    for (int i = 0; i < V; i++) {
        if (!vis[i]) {
            count++;
            dfs(i, adj, vis);
        }
    }
    return count;
}
```

## New Keywords / STL Used
- `vector`

## Edge Cases
- graph with 0 edges, fully connected graph, graph with multiple disconnected subgraphs.

NEXT: [[Index]]
