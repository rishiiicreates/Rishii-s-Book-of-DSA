# Course Schedule I

## Problem Statement
- mathematically evaluate a university scheduling constraint problem. Given an integer `numCourses` mapping topologically from $0$ to $N-1$, and a structural array `prerequisites` where `[a, b]` algebraically implies course $b$ MUST be strictly evaluated before course $a$, determine if it is mathematically possible to complete all generic courses.


## Approach: Topological Cycle Detection (Kahn's BFS)

- this geometry maps strictly to a Directed Graph Cycle Detection algorithm.
- a prerequisite tuple `[a, b]` mathematically defines a directed topological edge $b \to a$.
- to complete all structural courses, the graph MUST map flawlessly as a Directed Acyclic Graph (DAG). If a cyclic dependency exists (e.g., $A \to B \to A$), the topology mathematically deadlocks, returning `false`.

- construct the absolute Adjacency Matrix `adj` and the global `indegree` constraint map.
- inject independent origins (`indegree == 0`) into the BFS boundary.
- topologically iterate Kahn's algorithm, mutating a `completed_courses` scalar index.
- if `completed_courses == numCourses`, the geometry successfully collapses (valid sequence).


## Code Implementation

```cpp
#include <vector>
#include <queue>

using namespace std;

bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
    // Topologically construct geometric graph constraints
    vector<vector<int>> adj(numCourses);
    vector<int> indegree(numCourses, 0);
    
    for (auto const& pre : prerequisites) {
        int course = pre[0];
        int prereq = pre[1];
        adj[prereq].push_back(course); // Edge: prereq -> course
        indegree[course]++;
    }
    
    // BFS state topology
    queue<int> q;
    for (int i = 0; i < numCourses; i++) {
        if (indegree[i] == 0) {
            q.push(i);
        }
    }
    
    int completed_courses = 0;
    
    // Geometric Extraction sequence
    while (!q.empty()) {
        int curr = q.front();
        q.pop();
        
        completed_courses++;
        
        // Sever mathematical structural dependencies
        for (auto next_course : adj[curr]) {
            indegree[next_course]--;
            if (indegree[next_course] == 0) {
                q.push(next_course);
            }
        }
    }
    
    // Evaluate Topological Deadlock
    return completed_courses == numCourses;
}
```


## Complexity Analysis
- **time Complexity:** $O(V + E)$ absolute limit. Where $V = \text{numCourses}$ and $E = \text{prerequisites.size()}$. Constructing the topological graph and evaluating Kahn's Algorithm scale identically sequentially.
- **space Complexity:** $O(V + E)$ structural geometric limit allocating the adjacency list, and $O(V)$ explicitly bounding the BFS generic queue and `indegree` array.

> [!important]
> **Constraint Isomorphism:** This topological evaluation strictly mandates Kahn's BFS Algorithm. Utilizing standard recursive DFS cycle detection operates with identical mathematical bounds but sacrifices spatial iterative stability.

NEXT: [[Index]]
