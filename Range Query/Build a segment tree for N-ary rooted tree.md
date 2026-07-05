# Build a segment tree for N-ary rooted tree

## Problem Statement
- perform range queries and updates on subtrees or paths in an N-ary rooted tree.

## Approach / Intuition
- by using an [[Euler Tour]] (or DFS flattening), we can map the nodes of the tree to a 1D array where a subtree corresponds to a contiguous subarray. Then, we can use a standard [[Segment Tree]] over this flattened array.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$ for Euler tour, $O(\log N)$ for queries
- **[[space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <vector>

using namespace std;

class TreeSegmentTree {
    int timer = 0;
    vector<int> tin, tout;
    vector<int> flattenedTree;

    void dfs(int u, int p, const vector<vector<int>>& adj, const vector<int>& vals) {
        tin[u] = ++timer;
        flattenedTree[timer] = vals[u];
        for (int v : adj[u]) {
            if (v != p) dfs(v, u, adj, vals);
        }
        tout[u] = timer;
    }

public:
    TreeSegmentTree(int n, const vector<vector<int>>& adj, const vector<int>& vals) {
        tin.resize(n + 1);
        tout.resize(n + 1);
        flattenedTree.resize(n + 1);
        dfs(1, 0, adj, vals);
        
        // build segment tree using `flattenedTree` array from index 1 to `timer`
    }
    
    pair<int, int> getSubtreeRange(int u) {
        return {tin[u], tout[u]};
    }
};
```

## New Keywords / STL Used
- `std::pair`

## Edge Cases
- leaf nodes, skewed tree (line graph), tree with only one node.

NEXT: [[Index]]
