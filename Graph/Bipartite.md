# Bipartite Graph

## Problem Statement
- determine if a graph is bipartite (i.e., its vertices can be divided into two disjoint sets such that no two adjacent vertices have the same color).

## Approach / Intuition
- use [[BFS]] or [[DFS]] for graph coloring. Color the starting node with 0 and all its neighbors with 1. If any neighbor is already colored with the same color, the graph is not bipartite.

## Time & Space Complexity
- **[[time Complexity]]:** O(V + E)
- **[[space Complexity]]:** O(V)

## Sample Code
```cpp
#include <vector>
#include <queue>
using namespace std;

bool check(int start, int V, vector<int> adj[], vector<int>& color) {
    queue<int> q;
    q.push(start);
    color[start] = 0;
    while(!q.empty()) {
        int node = q.front();
        q.pop();
        for(auto it : adj[node]) {
            if(color[it] == -1) {
                color[it] = !color[node];
                q.push(it);
            } else if(color[it] == color[node]) {
                return false;
            }
        }
    }
    return true;
}

bool isBipartite(int V, vector<int> adj[]) {
    vector<int> color(V, -1);
    for(int i = 0; i < V; i++) {
        if(color[i] == -1) {
            if(!check(i, V, adj, color)) return false;
        }
    }
    return true;
}
```

## New Keywords / STL Used
- `std::queue`, `std::vector`

## Edge Cases
- graph with odd length cycle (not bipartite), disconnected graph.

NEXT: [[Index]]
