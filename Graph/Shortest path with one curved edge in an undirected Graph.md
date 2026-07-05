# Shortest path with one curved edge in an undirected Graph

## Problem Statement
- find the shortest path between two nodes in an undirected graph where you can traverse exactly one alternate "curved" edge instead of a normal edge.

## Approach / Intuition
- run [[Dijkstra]] from the source node to get shortest distances array, and run it again from the destination node to get a second distances array. Iterate over all curved edges and find the minimum total path considering the curved edge in both orientations.

## Time & Space Complexity
- **[[time Complexity]]:** O((V + E) \log V)
- **[[space Complexity]]:** O(V + E)

## Sample Code
```cpp
int shortestPathWithCurvedEdge(int n, int m, int a, int b, vector<vector<int>>& edges, vector<vector<int>>& curvedEdges) {
    vector<pair<int, int>> adj[n + 1];
    for(auto& edge : edges) {
        adj[edge[0]].push_back({edge[1], edge[2]});
        adj[edge[1]].push_back({edge[0], edge[2]});
    }
    auto dijkstra = [&](int src) {
        vector<int> dist(n + 1, 1e9);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        dist[src] = 0;
        pq.push({0, src});
        while(!pq.empty()) {
            int d = pq.top().first;
            int u = pq.top().second;
            pq.pop();
            if(d > dist[u]) continue;
            for(auto& edge : adj[u]) {
                int v = edge.first;
                int w = edge.second;
                if(dist[u] + w < dist[v]) {
                    dist[v] = dist[u] + w;
                    pq.push({dist[v], v});
                }
            }
        }
        return dist;
    };
    vector<int> distA = dijkstra(a);
    vector<int> distB = dijkstra(b);
    int ans = distA[b];
    for(auto& edge : curvedEdges) {
        int u = edge[0], v = edge[1], w = edge[2];
        ans = min({ans, distA[u] + w + distB[v], distA[v] + w + distB[u]});
    }
    return ans >= 1e9 ? -1 : ans;
}
```

## New Keywords / STL Used
- `auto`, lambda functions, `min` with initializer list

## Edge Cases
- no curved edges provided, using no curved edges is optimal, source and destination are the same.

NEXT: [[Index]]
