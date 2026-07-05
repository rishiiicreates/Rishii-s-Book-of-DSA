# Minimizing Infection Time in a Directed Graph

## Problem Statement
- given a directed graph with edge weights representing infection delay, find the minimum time to infect all nodes starting from a specific node, or return -1 if impossible.

## Approach / Intuition
- this is equivalent to finding the shortest path to all nodes and taking the maximum. We can apply [[Dijkstra]] algorithm to calculate shortest paths from the source to all nodes.

## Time & Space Complexity
- **[[time Complexity]]:** O(E \log V)
- **[[space Complexity]]:** O(V + E)

## Sample Code
```cpp
int networkDelayTime(vector<vector<int>>& times, int n, int k) {
    vector<pair<int, int>> adj[n + 1];
    for (auto& time : times) {
        adj[time[0]].push_back({time[1], time[2]});
    }
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    vector<int> dist(n + 1, 1e9);
    dist[k] = 0;
    pq.push({0, k});
    while (!pq.empty()) {
        int time = pq.top().first;
        int node = pq.top().second;
        pq.pop();
        for (auto& edge : adj[node]) {
            int adjNode = edge.first;
            int wt = edge.second;
            if (time + wt < dist[adjNode]) {
                dist[adjNode] = time + wt;
                pq.push({dist[adjNode], adjNode});
            }
        }
    }
    int mx = 0;
    for (int i = 1; i <= n; i++) {
        if (dist[i] == 1e9) return -1;
        mx = max(mx, dist[i]);
    }
    return mx;
}
```

## New Keywords / STL Used
- `priority_queue`, `pair`, `greater`

## Edge Cases
- disconnected nodes (return -1), tree structure, densely connected graph.

NEXT: [[Index]]
