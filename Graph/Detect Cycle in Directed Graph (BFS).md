---
type: concept
tags: [graph, bfs, cycle-detection, cpp, dag, kahn]
date: 2026-07-01
---
# Detect Cycle in Directed Graph (BFS)

## Problem Statement
Given a generic Directed Graph mapping $V$ topological vertices and $E$ directed edges, mathematically prove the structural existence of at least one directed cycle geometry.

---

## Approach: Kahn's Algorithm Anomaly (Topological Sort)

A **Topological Sort** is a mathematically exclusive property of a Directed **Acyclic** Graph (DAG). If a directed graph contains a cycle, executing a topological sort structurally fails to extract all nodes.
We leverage Kahn's BFS Algorithm identically:
1. Map the algebraic in-degree bounds for all $V$.
2. Inject `0` in-degree nodes into the BFS frontier queue.
3. Topologically mutate the queue, incrementing a structural scalar `count` for every valid extraction, and mathematically sever outbound vectors, injecting new `0` in-degree nodes.
4. **The Theorem:** If the graph operates devoid of cycles (is a strict DAG), Kahn's Algorithm will successfully map exactly $V$ nodes (`count == V`). If a cycle boundary exists, mutual cyclic dependencies freeze their in-degrees mathematically $\ge 1$, terminating the BFS prematurely (`count < V`).

---

## Code Implementation

```cpp
#include <vector>
#include <queue>

using namespace std;

bool isCyclic(int V, vector<int> adj[]) {
    vector<int> indegree(V, 0);
    
    // Geometric absolute inbound vector calculation
    for (int i = 0; i < V; i++) {
        for (auto it : adj[i]) {
            indegree[it]++;
        }
    }
    
    queue<int> q;
    
    // Inject absolute independent topologies
    for (int i = 0; i < V; i++) {
        if (indegree[i] == 0) {
            q.push(i);
        }
    }
    
    int processed_nodes = 0;
    
    // Topological descent
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        processed_nodes++;
        
        // Subjugate outbound dependencies
        for (auto it : adj[node]) {
            indegree[it]--;
            if (indegree[it] == 0) {
                q.push(it);
            }
        }
    }
    
    // Algebraic Anomaly Isolation
    return (processed_nodes != V);
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(V + E)$ spatial bound limit. Structurally identical to Kahn's Algorithm, mapping complete geometric traversal.
- **Space Complexity:** $O(V)$ tracking explicit inbound constraints (`indegree` array) and bounding the active BFS queue.

> [!tip]
> **DFS Back-Edge Isomorphism:** A Depth-First Search maps identical topological detection via `visited` and `path_visited` state arrays $O(V)$. However, Kahn's algorithm bypasses call-stack constraints and elegantly recycles fundamental Topological Sorting topology.
