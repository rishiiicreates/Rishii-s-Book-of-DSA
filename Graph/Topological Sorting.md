---
type: concept
tags: [graph, cpp, topological-sort]
date: 2026-06-30
---
# Topological Sorting

## Problem Statement
Find a linear ordering of vertices such that for every directed edge u -> v, vertex u comes before v in the ordering. Valid only for [[Directed Acyclic Graph]] (DAG).

## Approach / Intuition
Use Kahn's Algorithm ([[BFS]] with in-degree array) or [[DFS]] with a [[Stack]]. In DFS, after recursively visiting all neighbors, push the node into a stack.

## Time & Space Complexity
- **[[Time Complexity]]:** O(V + E)
- **[[Space Complexity]]:** O(V)

## Sample Code
```cpp
#include <vector>
#include <stack>
using namespace std;

void dfs(int node, vector<int> adj[], vector<bool>& vis, stack<int>& st) {
    vis[node] = true;
    for(auto it : adj[node]) {
        if(!vis[it]) dfs(it, adj, vis, st);
    }
    st.push(node);
}

vector<int> topoSort(int V, vector<int> adj[]) {
    vector<bool> vis(V, false);
    stack<int> st;
    for(int i = 0; i < V; i++) {
        if(!vis[i]) dfs(i, adj, vis, st);
    }
    vector<int> ans;
    while(!st.empty()) {
        ans.push_back(st.top());
        st.pop();
    }
    return ans;
}
```

## New Keywords / STL Used
`std::stack`

## Edge Cases
Disconnected DAG, single node, graph with cycle (not applicable).
