---
type: concept
tags: [graph, cpp, bfs]
date: 2026-06-30
---
# Breadth First Search (BFS)

## Problem Statement
Traverse or search tree or graph data structures starting from a given source node, exploring neighbor nodes level by level.

## Approach / Intuition
Use a [[Queue]] to keep track of nodes to visit next and a boolean array to mark visited nodes to prevent cycles. Process each level completely before moving to the next.

## Time & Space Complexity
- **[[Time Complexity]]:** O(V + E)
- **[[Space Complexity]]:** O(V)

## Sample Code
```cpp
#include <vector>
#include <queue>
using namespace std;

vector<int> bfsOfGraph(int V, vector<int> adj[]) {
    vector<int> bfs;
    vector<bool> vis(V, false);
    queue<int> q;
    q.push(0);
    vis[0] = true;
    while(!q.empty()) {
        int node = q.front();
        q.pop();
        bfs.push_back(node);
        for(auto it : adj[node]) {
            if(!vis[it]) {
                vis[it] = true;
                q.push(it);
            }
        }
    }
    return bfs;
}
```

## New Keywords / STL Used
`std::queue`, `std::vector`

## Edge Cases
Disconnected graph, single node, empty graph.
