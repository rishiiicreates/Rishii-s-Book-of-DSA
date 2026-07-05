# M-Coloring Problem

## Problem Statement
- determine if it's possible to color the vertices of a graph using at most $M$ colors such that no two adjacent vertices share the same color.

## Approach / Intuition
- use [[Backtracking]] to assign colors to vertices one by one. For a current vertex, try all $M$ colors. Before assigning a color, check if it's safe (i.e., no adjacent vertex in the [[Graph]] has the same color). If safe, assign it and recurse for the next vertex. If all subsequent vertices can be colored, return true; else, backtrack and try the next color.

## Time & Space Complexity
- **[[time Complexity]]:** $O(M^V)$ where $V$ is the number of vertices
- **[[space Complexity]]:** $O(V)$ for the color array and recursion stack

## Sample Code
```cpp
#include <vector>

using namespace std;

bool isSafe(int node, int color[], vector<vector<int>>& graph, int n, int c) {
    for (int i = 0; i < n; i++) {
        if (graph[node][i] && color[i] == c) return false;
    }
    return true;
}

bool solve(int node, int color[], vector<vector<int>>& graph, int m, int n) {
    if (node == n) return true;
    
    for (int i = 1; i <= m; i++) {
        if (isSafe(node, color, graph, n, i)) {
            color[node] = i;
            if (solve(node + 1, color, graph, m, n)) return true;
            color[node] = 0;
        }
    }
    return false;
}

bool graphColoring(vector<vector<int>>& graph, int m, int n) {
    int color[101] = {0};
    return solve(0, color, graph, m, n);
}
```

## New Keywords / STL Used
- adjacency [[Matrix]] traversal

## Edge Cases
- graph with no edges, complete graph (needs exactly $V$ colors), disconnected components.

NEXT: [[Index]]
