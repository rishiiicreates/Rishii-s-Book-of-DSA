---
type: concept
tags: [graph, cpp, shortest-path, dijkstra]
date: 2026-06-30
---
# Dijkstra

## Problem Statement
Find the shortest path from a single source to all other vertices in a weighted graph with non-negative edge weights.

## Approach / Intuition
Maintain a [[Priority Queue]] (min-heap) to greedily pick the vertex with the smallest distance. Relax its neighbors by checking if the path through this vertex offers a shorter distance.

## Time & Space Complexity
- **[[Time Complexity]]:** O((V + E) \log V)
- **[[Space Complexity]]:** O(V + E)

## Sample Code
```cpp
vector<int> dijkstra(int V, vector<vector<int>> adj[], int S) {
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    vector<int> dist(V, 1e9);
    dist[S] = 0;
    pq.push({0, S});
    while(!pq.empty()) {
        int dis = pq.top().first;
        int node = pq.top().second;
        pq.pop();
        for(auto it : adj[node]) {
            int edgeWeight = it[1];
            int adjNode = it[0];
            if(dis + edgeWeight < dist[adjNode]) {
                dist[adjNode] = dis + edgeWeight;
                pq.push({dist[adjNode], adjNode});
            }
        }
    }
    return dist;
}
```

## New Keywords / STL Used
`priority_queue`, `pair`, `vector`, `greater`

## Edge Cases
Disconnected graph, single node graph, graph with 0 weight edges.
