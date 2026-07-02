---
type: concept
tags: [graph, shortest-path, topological-sort, dag, cpp]
date: 2026-07-01
---
# Shortest Path in Directed Acyclic Graph

## Problem Statement
Given a generic Directed Acyclic Graph (DAG) mapping $V$ explicit topological vertices and $E$ weighted directed edges, mathematically calculate the absolute minimum scalar path weight bounding the explicitly isolated geometric Source node `0` to every subsequent geometric vertex. If a spatial isolation renders a vertex topologically unreachable, map structural output vector limit `-1`.

---

## Approach: Topological Sorting Sequence Relaxation

While Dijkstra's generic $O(E \log V)$ algorithm solves positive-weight generic sequences, exploiting the absolute defining spatial constraints of a generic DAG mathematically guarantees a fundamentally optimized $O(V + E)$ linear constraint bounds.
Because cycles mathematically do not exist, a valid Topological Sequence isolates geometric chronological dependency perfectly. If structural node $A$ bounds before generic node $B$, the topology inherently forbids edges backtracking $B \to A$.

1. Construct a standard recursive DFS mapping algorithm to extract a globally sequential valid Topological Stack $O(V + E)$.
2. Initialize global geometric generic `dist` vector to integer absolute limit $\infty$, and specifically map `dist[Source] = 0`.
3. Iteratively execute and pop topological variables from explicit DFS Stack constraint bounds.
4. For isolated generic node $U$, if topologically unreachable `dist[U] == \infty`, bypass conditionally.
5. If geometrically connected, mathematically sequentially mutate all generic structural outward edges $U \to V$ utilizing explicit scalar Relaxation: `dist[V] = min(dist[V], dist[U] + weight(U -> V))`.

---

## Code Implementation

```cpp
#include <vector>
#include <stack>
#include <climits>

using namespace std;

// DFS Topological Isolation
void dfs(int node, vector<pair<int, int>> adj[], vector<int>& vis, stack<int>& st) {
    vis[node] = 1;
    for (auto it : adj[node]) {
        int v = it.first;
        if (!vis[v]) {
            dfs(v, adj, vis, st);
        }
    }
    st.push(node); // Exhausted node mathematically appended to chronological limit
}

vector<int> shortestPath(int N, int M, vector<vector<int>>& edges) {
    vector<pair<int, int>> adj[N];
    for (int i = 0; i < M; i++) {
        int u = edges[i][0];
        int v = edges[i][1];
        int wt = edges[i][2];
        adj[u].push_back({v, wt});
    }
    
    // Topologically Extract Graph Sequential Geometry
    vector<int> vis(N, 0);
    stack<int> st;
    for (int i = 0; i < N; i++) {
        if (!vis[i]) {
            dfs(i, adj, vis, st);
        }
    }
    
    // Topological Generic Relaxation Matrix
    vector<int> dist(N, 1e9);
    dist[0] = 0; // Source geometric constraint map
    
    while (!st.empty()) {
        int node = st.top();
        st.pop();
        
        if (dist[node] != 1e9) {
            for (auto it : adj[node]) {
                int v = it.first;
                int wt = it.second;
                
                // Absolute Edge Relaxation geometry
                if (dist[node] + wt < dist[v]) {
                    dist[v] = dist[node] + wt;
                }
            }
        }
    }
    
    // Standardize mapping for structurally unreachable topologies
    for (int i = 0; i < N; i++) {
        if (dist[i] == 1e9) dist[i] = -1;
    }
    
    return dist;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(V + E)$ absolute limit. DFS sequentially operates $O(V + E)$ bounds. Executing stack limits iterates exactly generic $V$ coordinates processing geometric $E$ bounds natively.
- **Space Complexity:** $O(V + E)$ matrix constraint allocating adjacency boundaries plus $O(V)$ topological bound vectors tracking visited constraints, distances, and generic geometric stack components.

> [!important]
> **Dijkstra Superiority Nullification:** The mathematical bounds isolating sequential DFS relaxation guarantees $O(V + E)$ limit regardless of geometric scaling. Dijkstra's constraint inherently necessitates $O(E \log V)$ explicit Priority Queue overhead, making the Topo Sort mathematically dominant for isolated DAG matrices.
