# Course Schedule II

## Problem Statement
- structurally expand the *Course Schedule I* topology constraint. Given an integer `numCourses` (mapping $0$ to $N-1$) and a structural prerequisite geometry `[a, b]` (edge $b \to a$), mathematically construct a valid sequential execution vector (Topological Sort). If multiple valid geometries exist, output any one sequence. If a topological cycle deadlocks the matrix, output an empty vector `[]`.


## Approach: Kahn's Algorithm (Topological Reification)

- this is an explicit reification of the Topological Sort state topology. Instead of merely aggregating a validation scalar `completed_courses`, we must mathematically serialize the specific vertex scalars into a localized vector structure.

- construct the geometric Adjacency List `adj` and global `indegree` limits identically $O(V+E)$.
- queue all algebraically independent origin vectors (`indegree == 0`).
- during topological BFS extraction, explicitly push the processed vertex sequence into an output `ans` vector.
- if a generic cycle freezes the geometry, Kahn's algorithm isolates premature exhaustion. If `ans.size() != numCourses`, mathematically return an empty set `{}`. Otherwise, return `ans`.


## Code Implementation

```cpp
#include <vector>
#include <queue>

using namespace std;

vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
    // Construct Topological Geometry
    vector<vector<int>> adj(numCourses);
    vector<int> indegree(numCourses, 0);
    
    for (auto const& pre : prerequisites) {
        int course = pre[0];
        int prereq = pre[1];
        adj[prereq].push_back(course); // Directed edge: prereq -> course
        indegree[course]++;
    }
    
    queue<int> q;
    for (int i = 0; i < numCourses; i++) {
        if (indegree[i] == 0) {
            q.push(i);
        }
    }
    
    vector<int> topo_order;
    
    // Sequential State Serialization
    while (!q.empty()) {
        int curr = q.front();
        q.pop();
        
        topo_order.push_back(curr);
        
        // Destruct structural bound limits
        for (auto next_course : adj[curr]) {
            indegree[next_course]--;
            if (indegree[next_course] == 0) {
                q.push(next_course);
            }
        }
    }
    
    // Verify Anomaly Exhaustion
    if (topo_order.size() == numCourses) {
        return topo_order;
    }
    
    return {}; // Mathematical topological deadlock -> sequence impossible
}
```


## Complexity Analysis
- **time Complexity:** $O(V + E)$ where $V = \text{numCourses}$ bounding vertices and $E = \text{prerequisites.size()}$ bounding structural edges.
- **space Complexity:** $O(V + E)$ mapping explicit absolute dimensions for the adjacency list structure, $O(V)$ explicitly isolating output geometries and constraints.

> [!tip]
> **DFS Reversal Theorem:** To construct topological sequencing utilizing recursive DFS, one must algebraically push structurally exhausted nodes (all descendants visited) into a `stack`. Popping the global stack subsequently generates the exact forward monotonic Topological geometry. BFS Kahn's Algorithm produces it natively sequentially.

NEXT: [[Index]]
