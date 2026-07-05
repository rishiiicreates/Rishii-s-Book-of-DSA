# Number of Provinces (Connected Components)

## Problem Statement
- there are $N$ cities. Some are statically connected, while others are isolated. If city $A$ is topologically connected to city $B$, and city $B$ is connected to city $C$, then city $A$ is transitively connected to city $C$.

- a **Province** (or mathematically, a **Connected Component**) is defined as a maximal group of directly or transitively connected cities. Given an $N \times N$ adjacency matrix `isConnected` where `isConnected[i][j] = 1` implies an edge between $i$ and $j$, rigorously compute the total number of distinct provinces.


## Approach: Global Component Exhaustion via DFS

- this problem mathematically translates to counting the absolute number of Disjoint Connected Components in an undirected graph.

- whenever we initialize a [[DFS in Graph]] (or BFS) from an unvisited vertex $V$, that traversal is mathematically guaranteed to explore and mark every single vertex within $V$'s isolated component. Therefore, the quantity of total components strictly equals the number of times we are forced to re-trigger a fresh DFS from the global vertex pool.

- **topological Mapping:** The input is provided as an Adjacency Matrix. To optimize edge traversal down to $O(V+E)$, we first convert the $O(V^2)$ matrix into an Adjacency List. *(Note: You can run DFS directly on the matrix, but doing so forces an $O(V)$ scan per node, yielding $O(V^2)$ time even for sparse graphs).*
- **state Tracking:** Initialize a `visited` array of size $N$, defaulting to `false`. Initialize a scalar counter `provinces = 0`.
- **component Iteration:**
   - iterate mathematically through all vertices $0 \dots N-1$.
   - if a vertex $i$ evaluates as `!visited[i]`:
     - increment `provinces++`.
     - trigger a `DFS(i)` to exhaustively mark the entire component as `visited`.
- **return:** `provinces`.


## Code Implementation

```cpp
#include <vector>
using namespace std;

// Standard DFS descent
void dfs(int node, const vector<vector<int>>& adj, vector<bool>& visited) {
    visited[node] = true;
    for (int neighbor : adj[node]) {
        if (!visited[neighbor]) {
            dfs(neighbor, adj, visited);
        }
    }
}

int findCircleNum(vector<vector<int>>& isConnected) {
    int n = isConnected.size();
    vector<vector<int>> adj(n);
    
    // Phase 1: Matrix to List Transformation
    // Omitting self-loops (i == j) simplifies traversal logic
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (i != j && isConnected[i][j] == 1) {
                adj[i].push_back(j);
            }
        }
    }
    
    // Phase 2: Component Enumeration
    vector<bool> visited(n, false);
    int provinces = 0;
    
    for (int i = 0; i < n; ++i) {
        // Discovered a previously uncharted topology
        if (!visited[i]) {
            provinces++;
            dfs(i, adj, visited); // Exhaust the component
        }
    }
    
    return provinces;
}
```

> [!tip]
> This problem can also be natively solved using a [[Disjoint Set]] (Union-Find) data structure. Initialize $N$ disjoint sets, iterate over the matrix, and execute `Union(i, j)`. The final number of provinces mathematically equals the number of unique root parents remaining. 


## Complexity Analysis
- **time Complexity:** $O(V^2)$. Constructing the Adjacency List strictly mandates scanning the entire $V \times V$ input matrix. The subsequent DFS exhaustion phase computes in bounded $O(V + E)$ time. Because $E \le V^2$, the construction phase dominates, locking the asymptotic bound to $O(V^2)$.
- **space Complexity:** $O(V^2)$ auxiliary space. The Adjacency List can dynamically scale to $O(V^2)$ elements in a fully connected dense graph. The `visited` array requires $O(V)$, and the recursive DFS stack peaks at $O(V)$.

NEXT: [[Index]]
