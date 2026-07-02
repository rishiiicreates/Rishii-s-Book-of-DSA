---
type: concept
tags: [graph, topological-sort, dfs, dag, cpp]
date: 2026-06-30
---
# Topological Sort (DFS)

## Mathematical Definition
A **Topological Sort** is a linear structural ordering of vertices in a **Directed Acyclic Graph (DAG)** such that for every directed edge $U \rightarrow V$, vertex $U$ strictly appears before vertex $V$ in the ordered sequence.

> [!warning]
> Topological sorting is mathematically impossible if the graph contains a cycle. It is exclusively defined for DAGs. It is heavily utilized for dependency resolution (e.g., Package Managers, Task Scheduling, Course Prerequisites).

---

## Approach: Post-Order DFS with Stack Reversal

The underlying mathematical intuition leverages the deep post-order phase of [[DFS in Graph]]. When a DFS traversal operating on a node $U$ reaches its terminal end (meaning $U$'s adjacency list is completely exhausted and all deep children have yielded), $U$ possesses no remaining topological dependencies.

Therefore, the node that finishes its DFS execution *first* is definitively a sink node (no outgoing dependencies). If we push every node onto a Stack at the exact moment its DFS function terminates, the Stack inherently accumulates the nodes in reverse-dependency order. Popping the Stack generates the correct topological sequence.

1. **State Preservation:** Maintain a `visited` boolean array and a `std::stack<int>`.
2. **Recursive Descent:**
   - Execute standard DFS on an unvisited node $U$.
   - Mark `visited[U] = true`.
   - Recurse deeply into all `!visited` adjacent neighbors $V$.
3. **Stack Push (Crucial Phase):**
   - After the adjacency loop concludes, the DFS branch for $U$ is mathematically exhausted.
   - `stack.push(U)`.
4. **Result Generation:** Pop elements from the stack sequentially into a result vector.

---

## Code Implementation

```cpp
#include <vector>
#include <stack>
using namespace std;

void dfsTopo(int node, vector<vector<int>>& adj, vector<bool>& visited, stack<int>& st) {
    // 1. Lock exploration state
    visited[node] = true;
    
    // 2. Aggressive recursive descent
    for (int neighbor : adj[node]) {
        if (!visited[neighbor]) {
            dfsTopo(neighbor, adj, visited, st);
        }
    }
    
    // 3. Post-Order execution logic
    // Node is topologically exhausted; its descendants are already processed
    st.push(node);
}

vector<int> topoSort(int V, vector<vector<int>>& adj) {
    vector<bool> visited(V, false);
    stack<int> st;
    vector<int> result;
    
    // Ensure all disconnected DAG components are covered
    for (int i = 0; i < V; ++i) {
        if (!visited[i]) {
            dfsTopo(i, adj, visited, st);
        }
    }
    
    // Reverse the terminal stack to establish valid chronological ordering
    while (!st.empty()) {
        result.push_back(st.top());
        st.pop();
    }
    
    return result;
}
```

> [!tip]
> **Kahn's Algorithm (BFS)** is the dominant alternative to this approach, utilizing In-Degree mapping. Kahn's Algorithm has the secondary structural benefit of natively detecting cycles during execution, whereas this DFS assumes the graph is unconditionally a DAG.

---

## Complexity Analysis
- **Time Complexity:** $O(V + E)$. The DFS comprehensively explores every single node and directed edge exactly once. Pushing to and popping from the stack operate in strict $O(1)$ constant time.
- **Space Complexity:** $O(V)$ auxiliary space. The `visited` array, the result stack, and the recursive call depth collectively bound the spatial requirement to a linear factor of vertices.
