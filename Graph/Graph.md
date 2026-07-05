# Graph Data Structure

## Mathematical Definition
- a **Graph** $G$ is mathematically formalized as an ordered pair $G = (V, E)$, where:
- $v$ is a finite set of **Vertices** (or nodes).
- $e \subseteq V \times V$ is a set of **Edges** connecting pairs of vertices.

### Taxonomies
- **directed vs. Undirected:** If the set of edges $E$ consists of ordered pairs $(u, v)$ such that $(u, v) \ne (v, u)$, the graph is **Directed**. If the pairs are unordered $\{u, v\}$, it is **Undirected**.
- **weighted vs. Unweighted:** If a mathematical weight function $W: E \rightarrow \mathbb{R}$ maps every edge to a real number, the graph is **Weighted**.
- **cyclic vs. Acyclic:** If there exists a topological path that originates and terminates at the exact same vertex, the graph is **Cyclic**. (e.g., Directed Acyclic Graphs or DAGs).


## Memory Representations

- there are two primary paradigms for structuring graph data in memory, balancing the trade-offs between dense and sparse edge connectivity.

### 1. Adjacency Matrix
- a 2D array matrix $M$ of dimensions $|V| \times |V|$ where:
- $m[i][j] = 1$ (or weight $W$) if an edge $(i, j) \in E$.
- $m[i][j] = 0$ (or $\infty$) otherwise.

- **complexity:**
- **space:** $O(V^2)$
- **edge Query:** $O(1)$
- **neighbor Enumeration:** $O(V)$

> [!important]
> Adjacency matrices are catastrophic for sparse graphs (where $|E| \ll |V|^2$). They are practically exclusively reserved for ultra-dense topologies or algorithms fundamentally relying on matrix multiplication (e.g., Floyd-Warshall).

### 2. Adjacency List (Standard)
- an array (or hash map) of collections. `adj[u]` contains a dynamic list of all vertices $v$ such that $(u, v) \in E$.

- **complexity:**
- **space:** $O(V + E)$
- **edge Query:** $O(\text{degree}(u))$
- **neighbor Enumeration:** $O(\text{degree}(u))$


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

NEXT: [[Index]]
