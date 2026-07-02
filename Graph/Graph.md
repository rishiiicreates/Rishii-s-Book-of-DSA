---
type: concept
tags: [graph, representation, fundamental, cpp]
date: 2026-06-30
---
# Graph Data Structure

## Mathematical Definition
A **Graph** $G$ is mathematically formalized as an ordered pair $G = (V, E)$, where:
- $V$ is a finite set of **Vertices** (or nodes).
- $E \subseteq V \times V$ is a set of **Edges** connecting pairs of vertices.

### Taxonomies
1. **Directed vs. Undirected:** If the set of edges $E$ consists of ordered pairs $(u, v)$ such that $(u, v) \ne (v, u)$, the graph is **Directed**. If the pairs are unordered $\{u, v\}$, it is **Undirected**.
2. **Weighted vs. Unweighted:** If a mathematical weight function $W: E \rightarrow \mathbb{R}$ maps every edge to a real number, the graph is **Weighted**.
3. **Cyclic vs. Acyclic:** If there exists a topological path that originates and terminates at the exact same vertex, the graph is **Cyclic**. (e.g., Directed Acyclic Graphs or DAGs).

---

## Memory Representations

There are two primary paradigms for structuring graph data in memory, balancing the trade-offs between dense and sparse edge connectivity.

### 1. Adjacency Matrix
A 2D array matrix $M$ of dimensions $|V| \times |V|$ where:
- $M[i][j] = 1$ (or weight $W$) if an edge $(i, j) \in E$.
- $M[i][j] = 0$ (or $\infty$) otherwise.

**Complexity:**
- **Space:** $O(V^2)$
- **Edge Query:** $O(1)$
- **Neighbor Enumeration:** $O(V)$

> [!important]
> Adjacency matrices are catastrophic for sparse graphs (where $|E| \ll |V|^2$). They are practically exclusively reserved for ultra-dense topologies or algorithms fundamentally relying on matrix multiplication (e.g., Floyd-Warshall).

### 2. Adjacency List (Standard)
An array (or hash map) of collections. `adj[u]` contains a dynamic list of all vertices $v$ such that $(u, v) \in E$.

**Complexity:**
- **Space:** $O(V + E)$
- **Edge Query:** $O(\text{degree}(u))$
- **Neighbor Enumeration:** $O(\text{degree}(u))$

---

## C++ Adjacency List Implementation

```cpp
#include <vector>
#include <iostream>
using namespace std;

class Graph {
private:
    int V; // Cardinality of Vertices
    vector<vector<int>> adj; // Topological map

public:
    // O(V) Instantiation
    Graph(int V) {
        this->V = V;
        adj.resize(V);
    }
    
    // O(1) Amortized Insertion (Undirected)
    void addEdge(int u, int v) {
        adj[u].push_back(v);
        adj[v].push_back(u); // Remove for Directed Graph
    }
    
    // O(1) Retrieve adjacent nodes
    const vector<int>& getNeighbors(int u) const {
        return adj[u];
    }
};
```
