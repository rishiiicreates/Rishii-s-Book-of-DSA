---
type: concept
tags: [graph, topological-sort, bfs, cpp, dag, in-degree]
date: 2026-07-01
---
# Kahn's Algorithm (Topological Sort BFS)

## Problem Statement
Given a generic Directed Acyclic Graph (DAG) mapping $V$ topological vertices and $E$ directed edges, mathematically construct a linear sequence of the vertices such that for every directed geometric edge $U \to V$, the scalar vertex $U$ strictly precedes $V$ in the linear topological mapping.
This sequence algebraically defines a valid **Topological Sort**.

---

## Approach: In-Degree Geometric Induction (BFS)

Kahn's Algorithm constructs the topological ordering mathematically by isolating independent root nodes iteratively.
The fundamental theorem: A vertex with an **in-degree of 0** (no inbound structural edges) is mathematically independent of all other topological structures. It can unconditionally execute first.

1. Geometrically calculate the absolute in-degree for all vertices $O(V + E)$.
2. Inject all vertices mapping an in-degree strictly equal to `0` into a BFS queue constraint.
3. Iteratively execute the BFS topology:
   - Dequeue vertex $U$ and append it to the topological output geometry.
   - For every structural directed neighbor $N$ of $U$ (evaluating edge $U \to N$):
     - Mathematically sever the edge: decrement the in-degree of $N$ (`indegree[N]--`).
     - If the residual in-degree of $N$ collapses to `0`, $N$ is now topologically independent. Inject $N$ into the queue.
4. The topological geometry terminates when the BFS queue exhausts.

---

## Code Implementation

```cpp
#include <vector>
#include <queue>

using namespace std;

vector<int> topologicalSort(int V, vector<int> adj[]) {
    vector<int> indegree(V, 0);
    
    // Algebraically map absolute inbound topology
    for (int i = 0; i < V; i++) {
        for (auto it : adj[i]) {
            indegree[it]++;
        }
    }
    
    // BFS State geometry
    queue<int> q;
    
    // Inject independent root origins
    for (int i = 0; i < V; i++) {
        if (indegree[i] == 0) {
            q.push(i);
        }
    }
    
    vector<int> topo;
    
    // Topologically iterate sequence
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        topo.push_back(node);
        
        // Mathematically sever directed geometric edges
        for (auto it : adj[node]) {
            indegree[it]--;
            // Independence convergence
            if (indegree[it] == 0) {
                q.push(it);
            }
        }
    }
    
    return topo;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(V + E)$ absolute limit. Constructing the in-degree array evaluates all edges $O(V + E)$. The BFS queue maps every vertex strictly once, processing its outbound vectors.
- **Space Complexity:** $O(V)$ bounding the explicit `indegree` array and the BFS generic queue topology.

> [!important]
> **Cycle Anomaly Theorem:** If the graph inherently contains a directed cyclic boundary, the structural dependencies form an infinite geometric loop. Consequently, nodes within the cycle will mathematically NEVER reduce to an `in-degree == 0`. Kahn's Algorithm will terminate prematurely. If `topo.size() != V`, the geometry contains a cycle.
