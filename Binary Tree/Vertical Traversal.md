---
type: concept
tags: [binary-tree, cpp, math, geometry, bst]
date: 2026-07-01
---
# Vertical Order Traversal

## Problem Statement
Given a Binary Tree topology $T$, construct a geometrically ordered 2D sequence of its node scalars. The mathematical position of a node is defined by an absolute $(X, Y)$ coordinate space:
- Geometric root is at $(0, 0)$.
- Left child structurally maps to $(X - 1, Y + 1)$.
- Right child structurally maps to $(X + 1, Y + 1)$.

Output the scalars grouped primarily by $X$ coordinate (ascending), and secondarily by $Y$ coordinate (ascending). If multiple vertices occupy the exact same spatial matrix coordinate $(X, Y)$, they must be scalar-sorted ascending.

---

## Approach: Cartesian Coordinate Mapping & Multi-Set Geometry

The absolute constraints mathematically mandate a globally sorted 2D mapping topology. 
We evaluate a BFS traversal (or pre-order DFS) to inject every node into an ordered geometrical state map:
`map<int, map<int, multiset<int>>>`
1. The absolute outer `map` strictly sorts the discrete topological columns ($X$-axis).
2. The nested inner `map` strictly sorts the discrete topological rows ($Y$-axis).
3. The absolute inner `multiset` handles structural collision clustering, sorting scalar values that share identical $(X, Y)$ geometry automatically.

---

## Code Implementation

```cpp
#include <vector>
#include <map>
#include <set>
#include <queue>

using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

vector<vector<int>> verticalTraversal(TreeNode* root) {
    // Geometric State: map<X, map<Y, multiset<values>>>
    map<int, map<int, multiset<int>>> nodes;
    
    // BFS execution topological queue: {Node, {X, Y}}
    queue<pair<TreeNode*, pair<int, int>>> q;
    
    if (root != nullptr) {
        q.push({root, {0, 0}});
    }
    
    while (!q.empty()) {
        auto p = q.front();
        q.pop();
        
        TreeNode* curr = p.first;
        int x = p.second.first;
        int y = p.second.second;
        
        // Log scalar to exact geometric multi-set
        nodes[x][y].insert(curr->val);
        
        // Propagate geometric vectors
        if (curr->left) q.push({curr->left, {x - 1, y + 1}});
        if (curr->right) q.push({curr->right, {x + 1, y + 1}});
    }
    
    vector<vector<int>> ans;
    // Iterate ascending topological X columns
    for (auto const& p : nodes) {
        vector<int> col;
        // Iterate ascending topological Y rows
        for (auto const& q : p.second) {
            // Append multiset scalar block
            col.insert(col.end(), q.second.begin(), q.second.end());
        }
        ans.push_back(col);
    }
    
    return ans;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N \log N \cdot \log K)$ dominated heavily by dynamic insertion into the generic `std::map` and `std::multiset`.
- **Space Complexity:** $O(N)$ allocating absolute geometrical mappings for every discrete structural vertex.

> [!important]
> **BFS vs DFS Spatial Safety:** While both traversals evaluate valid topologies, BFS naturally explores monotonic $Y$ states sequentially, minimizing redundant state clustering. A pure DFS approach operates identically but requires the explicit $Y$-axis map, whereas BFS can sometimes implicitly guarantee $Y$-axis ordering if topological constraints allow skipping the inner map.
