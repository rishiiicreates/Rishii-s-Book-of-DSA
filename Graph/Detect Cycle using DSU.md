# Detect Cycle using DSU

## Problem Statement
- determine if an undirected graph contains a cycle.

## Approach / Intuition
- iterate over all edges in the graph. For each edge (u, v), if they already belong to the same component (have the same ultimate parent in the [[Disjoint Set]]), a cycle exists.

## Time & Space Complexity
- **[[time Complexity]]:** O(E \times \alpha(V))
- **[[space Complexity]]:** O(V)

## Sample Code
```cpp
class DisjointSet {
    vector<int> parent, size;
public:
    DisjointSet(int n) {
        parent.resize(n + 1);
        size.resize(n + 1, 1);
        for (int i = 0; i <= n; i++) parent[i] = i;
    }
    int findUPar(int node) {
        if (node == parent[node]) return node;
        return parent[node] = findUPar(parent[node]);
    }
    bool unionBySize(int u, int v) {
        int ulp_u = findUPar(u);
        int ulp_v = findUPar(v);
        if (ulp_u == ulp_v) return false;
        if (size[ulp_u] < size[ulp_v]) {
            parent[ulp_u] = ulp_v;
            size[ulp_v] += size[ulp_u];
        } else {
            parent[ulp_v] = ulp_u;
            size[ulp_u] += size[ulp_v];
        }
        return true;
    }
};
bool detectCycle(int V, vector<int> adj[]) {
    DisjointSet ds(V);
    for (int u = 0; u < V; u++) {
        for (int v : adj[u]) {
            if (u < v) {
                if (!ds.unionBySize(u, v)) {
                    return true;
                }
            }
        }
    }
    return false;
}
```

## New Keywords / STL Used
- `class`, `vector`

## Edge Cases
- empty graph, tree (no cycle), multi-graph with parallel edges, self loops.

NEXT: [[Index]]
