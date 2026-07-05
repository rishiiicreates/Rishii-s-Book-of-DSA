# Prim

## Problem Statement
- find the Minimum Spanning Tree (MST) of a connected, undirected graph with edge weights.

## Approach / Intuition
- use a [[Priority Queue]] (min-heap) to greedily select the edge with the smallest weight that connects a visited node to an unvisited node, successively adding it to the MST.

## Time & Space Complexity
- **[[time Complexity]]:** O(E \log V)
- **[[space Complexity]]:** O(V + E)

## Sample Code
```cpp
int spanningTree(int V, vector<vector<int>> adj[]) {
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    vector<int> vis(V, 0);
    pq.push({0, 0});
    int sum = 0;
    while (!pq.empty()) {
        int wt = pq.top().first;
        int node = pq.top().second;
        pq.pop();
        if (vis[node]) continue;
        vis[node] = 1;
        sum += wt;
        for (auto it : adj[node]) {
            int adjNode = it[0];
            int edW = it[1];
            if (!vis[adjNode]) {
                pq.push({edW, adjNode});
            }
        }
    }
    return sum;
}
```

## New Keywords / STL Used
- `priority_queue`, `greater`, `pair`

## Edge Cases
- disconnected graph, graph with single node, negative edge weights.

NEXT: [[Index]]
