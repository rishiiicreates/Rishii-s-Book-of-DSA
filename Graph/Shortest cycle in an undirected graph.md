---
type: concept
tags: [graph, cpp, cycle-detection, bfs]
date: 2026-06-30
---
# Shortest cycle in an undirected graph

## Problem Statement
Find the length of the shortest cycle in an unweighted undirected graph. If no cycle exists, return -1.

## Approach / Intuition
Run [[BFS]] from every node. During traversal, if we reach a visited node that is not the parent of the current node, a cycle is found. The cycle length will be the sum of distances of both nodes from the start node plus 1.

## Time & Space Complexity
- **[[Time Complexity]]:** O(V \times (V + E))
- **[[Space Complexity]]:** O(V + E)

## Sample Code
```cpp
int shortestCycle(int n, vector<vector<int>>& edges) {
    vector<int> adj[n];
    for (auto& e : edges) {
        adj[e[0]].push_back(e[1]);
        adj[e[1]].push_back(e[0]);
    }
    int ans = 1e9;
    for (int i = 0; i < n; i++) {
        vector<int> dist(n, 1e9);
        vector<int> parent(n, -1);
        queue<int> q;
        dist[i] = 0;
        q.push(i);
        while (!q.empty()) {
            int u = q.front();
            q.pop();
            for (int v : adj[u]) {
                if (dist[v] == 1e9) {
                    dist[v] = dist[u] + 1;
                    parent[v] = u;
                    q.push(v);
                } else if (parent[u] != v) {
                    ans = min(ans, dist[u] + dist[v] + 1);
                }
            }
        }
    }
    return ans == 1e9 ? -1 : ans;
}
```

## New Keywords / STL Used
`queue`, `vector`

## Edge Cases
Graph with no cycles (tree), disconnected graph, cycle of length 3 (triangle).
