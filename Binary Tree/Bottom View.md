---
type: concept
tags: [binary-tree, cpp, math, geometry, maps]
date: 2026-07-01
---
# Bottom View of a Binary Tree

## Problem Statement
Given a Binary Tree topology $T$, compute the strictly visible subset of node scalars when geometrically observing the tree structure from the absolute bottom boundary.
If multiple nodes mathematically occupy the identical vertical alignment (identical $X$-coordinate), strictly output the one resident at the maximum geometrical depth ($Y$-coordinate). For equivalent depth overlaps on the same axis, standard convention maps the structurally later (right-most traversed) node.

---

## Approach: Cartesian Axis Overwriting (BFS)

Similar to standard Vertical Traversal, we project the topology onto a 1D Cartesian Axis ($X$-axis).
However, unlike the absolute multi-set grouping, we structurally demand exactly ONE scalar per $X$ coordinate.
Because BFS inherently explores the tree mathematically level-by-level (strictly monotonic increasing $Y$), any subsequent geometric collision on a given $X$ coordinate is guaranteed to reside at a deeper (or identical) $Y$ level.
Thus, by strictly executing BFS and unconditionally *overwriting* the map boundary state for $X$, the terminal surviving state naturally holds the deepest topological vertex for every column.

---

## Code Implementation

```cpp
#include <vector>
#include <map>
#include <queue>

using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

vector<int> bottomView(TreeNode *root) {
    vector<int> ans;
    if (root == nullptr) return ans;
    
    // Geometrical 1D Mapping: map<X, value>
    map<int, int> mpp;
    
    // Topologically queue {Node, X}
    queue<pair<TreeNode*, int>> q;
    q.push({root, 0});
    
    while (!q.empty()) {
        auto it = q.front();
        q.pop();
        
        TreeNode* node = it.first;
        int x = it.second;
        
        // Unconditional Overwrite dictates deepest Y persistence
        mpp[x] = node->val;
        
        if (node->left != nullptr) {
            q.push({node->left, x - 1});
        }
        if (node->right != nullptr) {
            q.push({node->right, x + 1});
        }
    }
    
    // Collate topologically sorted X coordinates
    for (auto it : mpp) {
        ans.push_back(it.second);
    }
    
    return ans;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N \log X)$ where $X$ is the unique cardinality of horizontal dimensions. The `std::map` governs the $O(\log X)$ insertion boundary.
- **Space Complexity:** $O(N)$ bounding the BFS level geometry, plus $O(X)$ auxiliary storage for the Cartesian axis map.

> [!tip]
> **DFS Failure Anomaly:** While DFS structurally evaluates every node, its non-monotonic depth traversal fundamentally violates the overwrite safety mechanism. A deeper node might be overwritten by a shallower node evaluated later in the traversal sequence. BFS is mathematically mandatory to preserve terminal depth overwrite logic.
