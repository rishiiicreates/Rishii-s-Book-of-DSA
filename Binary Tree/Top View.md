# Top View of a Binary Tree

## Problem Statement
- given a Binary Tree topology $T$, compute the strictly visible subset of node scalars when geometrically observing the tree structure from the absolute top boundary.
- if multiple nodes mathematically occupy the identical vertical alignment (identical $X$-coordinate), strictly output the one resident at the minimum geometrical depth ($Y$-coordinate).


## Approach: Cartesian Axis Persistence (BFS)

- this is the geometric mathematical dual to the Bottom View topology. We project the binary tree structure onto a 1D horizontal Cartesian axis ($X$-axis).
- because a Breadth-First Search (BFS) mathematically explores the sequence in strictly monotonic increasing depth ($Y$), the absolute first topological node encountered on any explicit $X$ coordinate is guaranteed to geometrically sit at the shallowest depth.

- therefore, our state machine utilizes a generic `std::map`. When evaluating $X$:
- if $X$ is mathematically absent from the map, we inject the node scalar.
- if $X$ structurally exists, we completely ignore the current scalar, as it structurally resides at a geometrically inferior (deeper) topology.


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

vector<int> topView(TreeNode *root) {
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
        
        // Strict Geometric Persistence 
        // Only evaluate if X is topologically vacant
        if (mpp.find(x) == mpp.end()) {
            mpp[x] = node->val;
        }
        
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


## Complexity Analysis
- **time Complexity:** $O(N \log X)$ mapping bounds. $N$ evaluations via BFS queue, multiplying a strict $O(\log X)$ `std::map` lookup/insertion topology, where $X$ denotes maximum absolute width dimensions.
- **space Complexity:** $O(N)$ max node boundary bounded by level recursion width constraints, and $O(X)$ tracking map density.

> [!important]
> **Depth Concealment Isolation:** Two disjoint nodes can never conceptually reside on the identical spatial coordinate $(X, Y)$ (except in generic multigraph structures). Hence, the strict $(X)$ axis persistence flawlessly insulates the geometry against structural shadow overlap, mapping exactly one vertex per geometric vertical boundary.

NEXT: [[Index]]
