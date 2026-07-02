---
type: concept
tags: [tree, binary-tree, graph, bfs, cpp, math]
date: 2026-06-30
---
# Nodes at Distance K

## Problem Statement
Given a discrete binary tree, a strict geometric target node pointer `target`, and a scalar distance variable $K$, extract all discrete mathematical nodes structurally residing exactly $K$ spatial unweighted geometric edges away from `target`.

---

## Approach: Undirected Graph Transformation (Parent Pointers + BFS)

A native binary tree strictly limits topological boundaries to one-way downward causal spatial traversal. A radius of $K$ dictates isotropic structural explosion equally traversing children *and* structurally ascending explicitly towards parents. 
To facilitate symmetrical isotropic graph search, we must mathematically upgrade the unidirectional binary tree into a full unweighted bidirectional map.

Algorithm:
1. **Geometric Graph Transformation (DFS):** Penetrate the binary structure utilizing a standard DFS Preorder topology. As you scan, populate a structural `unordered_map<TreeNode*, TreeNode*>` physically mapping every child boundary backward exactly to its strict temporal parent.
2. **Isotropic Structural Blast (BFS):** Execute an undirected geometric Breadth-First Search (BFS) directly initiated at `target`. 
3. Employ a localized `unordered_set<TreeNode*>` explicitly preventing unbounded cyclical oscillations resulting from bidirectional mappings.
4. Structurally explode continuously outward by appending left, right, and explicitly evaluated parent pointers. 
5. Increment the mathematical spatial radius count. Once $r = K$, the strictly bounded elements currently residing globally inside the temporal BFS queue represent the definitive geometric result space.

---

## Code Implementation

```cpp
#include <vector>
#include <queue>
#include <unordered_map>
#include <unordered_set>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

// Map structural dependencies bottom-up
void mapParents(TreeNode* node, unordered_map<TreeNode*, TreeNode*>& parent_map) {
    if (node == nullptr) return;
    
    if (node->left) {
        parent_map[node->left] = node; // Geometrically bind
        mapParents(node->left, parent_map);
    }
    if (node->right) {
        parent_map[node->right] = node; // Geometrically bind
        mapParents(node->right, parent_map);
    }
}

vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
    unordered_map<TreeNode*, TreeNode*> parent_map;
    mapParents(root, parent_map); // Transform to Bidirectional structure
    
    unordered_set<TreeNode*> visited;
    queue<TreeNode*> q;
    
    q.push(target);
    visited.insert(target); // Force structural anti-oscillation
    
    int current_radius = 0;
    
    // Isotropic spatial boundary expansion
    while (!q.empty()) {
        if (current_radius == k) break; // Absolute radius geometrically reached
        
        int level_size = q.size();
        for (int i = 0; i < level_size; i++) {
            TreeNode* current = q.front();
            q.pop();
            
            // Blast Downwards Left
            if (current->left && visited.find(current->left) == visited.end()) {
                visited.insert(current->left);
                q.push(current->left);
            }
            // Blast Downwards Right
            if (current->right && visited.find(current->right) == visited.end()) {
                visited.insert(current->right);
                q.push(current->right);
            }
            // Blast Upwards (Parent Tracking)
            if (parent_map.find(current) != parent_map.end() && visited.find(parent_map[current]) == visited.end()) {
                visited.insert(parent_map[current]);
                q.push(parent_map[current]);
            }
        }
        current_radius++;
    }
    
    vector<int> res;
    while (!q.empty()) {
        res.push_back(q.front()->val);
        q.pop();
    }
    
    return res;
}
```

> [!important]
> For highly dense geometric clusters where allocating $\mathcal{O}(N)$ parent tracking pointers induces extreme memory bounds, a pure dual-pass Recursive backtracking equivalent functionally calculates exact geometry devoid of maps, albeit dramatically sacrificing theoretical comprehension and linear clarity.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. Physical initialization bounds graph transformation at $N$ edges. The spatial isotropic BFS visits geometry exclusively strictly once.
- **Space Complexity:** $\mathcal{O}(N)$. Total supplementary temporal mappings rigorously demand structural space mapped specifically exactly for all existent nodes.
