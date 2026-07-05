# Depth First Search (DFS)

## Problem Statement
- traverse a graph structure starting from a source node, exploring as far as possible along each branch before backtracking.

## Approach / Intuition
- use [[Recursion]] (implicit [[Stack]]) to explore nodes. Mark nodes as visited and recursively visit all unvisited neighbors.

## Time & Space Complexity
- **[[time Complexity]]:** O(V + E)
- **[[space Complexity]]:** O(V)

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
- `std::vector`

## Edge Cases
- disconnected components, deep graphs causing stack overflow, cycles.

NEXT: [[Index]]
