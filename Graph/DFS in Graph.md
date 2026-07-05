# Depth First Search (DFS) in Graph

## Problem Statement
- given a Graph $G = (V, E)$ and an initial source vertex $S$, mathematically traverse the graph by exploring as deeply as possible along each topological branch before backtracking.


## Approach: Stack-Based / Recursive Descent

- dFS fundamentally isolates a single topological path and pursues it to absolute exhaustion (either encountering a dead end or previously visited nodes) before recursively yielding control back up the call stack to explore lateral branches.

- similarly to [[BFS in Graph]], explicit cyclic protection via a `visited` state array is mathematically non-negotiable.

- **state Initialization:** Maintain a global or passed-by-reference boolean array `visited` of size $|V|$.
- **recursive Invariant:** For a generic recursive function `dfs(u)`:
   - immediately lock the topological state by marking `visited[u] = true`.
   - process vertex $u$ (e.g., store or print).
   - iterate linearly through all adjacent neighbors $V$ of $U$.
   - if a neighbor $V$ evaluates as mathematically undiscovered (`!visited[V]`), immediately recurse into `dfs(V)`.

> [!tip]
> While recursion is the standard implementation, an iterative DFS can be constructed using an explicit `std::stack`. However, iterative DFS behaves topologically differently regarding neighbor visitation order unless neighbors are specifically pushed in reverse order. The recursive mathematical definition is canonical.


## Code Implementation

```cpp
#include <vector>
#include <iostream>
using namespace std;

// Recursive descent module
void dfsUtil(int u, vector<vector<int>>& adj, vector<bool>& visited, vector<int>& res) {
    // 1. Lock topological state
    visited[u] = true;
    res.push_back(u);
    
    // 2. Depth propagation
    for (int v : adj[u]) {
        // Unexplored branch detected
        if (!visited[v]) {
            dfsUtil(v, adj, visited, res);
        }
    }
}

vector<int> dfsOfGraph(int V, vector<vector<int>>& adj, int startNode) {
    vector<int> res;
    vector<bool> visited(V, false);
    
    // Seed recursive descent
    dfsUtil(startNode, adj, visited, res);
    
    return res;
}
```

> [!warning]
> If the graph is mathematically **disconnected**, executing DFS from a single $S$ will only explore the connected component. To traverse a fully disconnected graph, iterate over all vertices from $0 \dots V-1$ and conditionally trigger `dfsUtil` if the vertex remains unvisited.


## Complexity Analysis
- **time Complexity:** $O(V + E)$. The recursive function `dfsUtil` is mathematically invoked exactly once per vertex ($O(V)$). Inside the function, the `for` loop iterates over the adjacency list, implying every edge is scanned exactly once per direction ($O(E)$ total).
- **space Complexity:** $O(V)$. The `visited` array rigorously requires $O(V)$ space. The recursive call stack expands linearly corresponding to the maximum depth of the topological branch. In pathological cases (e.g., a linked list topology), the recursion depth hits $O(V)$, demanding $O(V)$ auxiliary stack space.

NEXT: [[Index]]
