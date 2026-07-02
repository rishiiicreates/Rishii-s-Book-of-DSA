---
type: concept
tags: [binary-tree, dfs, cpp]
date: 2026-06-30
---
# Nodes without Siblings

## Problem Statement
Given a Binary Tree, mathematically identify and return a sorted array of all nodes that exist **without a sibling**. 

*Definition:* A sibling is defined topologically as a node sharing the exact same parent. Therefore, a node $N_i$ has no sibling if its parent possesses $N_i$ as its *only* child (i.e., either the left child is $N_i$ and the right child is `nullptr`, or vice versa).
*Mathematical Convention:* The root node is fundamentally excluded from this evaluation because it intrinsically lacks a parent.

---

## Approach: Parent-Centric Depth-First Search

To evaluate sibling relationships without requiring explicit parent pointers in the node struct, we must analyze the topology from the perspective of the **parent node** before stepping into the child recursion.

1. **Base Constraint:** If `node` is `nullptr`, terminate.
2. **Validation Logic (Parent Perspective):**
   - If `node->left != nullptr` AND `node->right == nullptr`, the left child is a topological orphan relative to sibling pairing. We record `node->left->data`.
   - If `node->right != nullptr` AND `node->left == nullptr`, the right child is a topological orphan. We record `node->right->data`.
3. **Recursive Descent:** Proceed to recursively analyze `node->left` and `node->right` via standard [[Depth First Search]].
4. **Post-Processing (Sorting):** The problem strictly requests the output in sorted order. Once the DFS exhausts the entire tree and populates our result vector, we apply $O(K \log K)$ sorting (where $K$ is the number of isolated nodes) to satisfy the constraint. If no such nodes exist, return `[-1]` by conventional problem definitions.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

void findOrphans(Node* root, vector<int>& res) {
    if (root == nullptr) {
        return;
    }
    
    // Case 1: Left child exists, Right child is mathematically void
    if (root->left != nullptr && root->right == nullptr) {
        res.push_back(root->left->data);
    }
    // Case 2: Right child exists, Left child is mathematically void
    else if (root->right != nullptr && root->left == nullptr) {
        res.push_back(root->right->data);
    }
    
    // Topologically propagate DFS
    findOrphans(root->left, res);
    findOrphans(root->right, res);
}

vector<int> noSibling(Node* root) {
    vector<int> res;
    findOrphans(root, res);
    
    if (res.empty()) {
        return {-1};
    }
    
    // Strict mathematical constraint requires monotonically increasing output
    sort(res.begin(), res.end());
    return res;
}
```

> [!warning]
> Do not attempt to check if `current_node` has a sibling from within `current_node` itself unless you artificially pass a boolean flag down from the parent indicating its sibling status. The parent-centric inspection `if (parent->left && !parent->right)` is mathematically far cleaner.

---

## Complexity Analysis
- **Time Complexity:** $O(N \log N)$. The DFS completes strictly in $O(N)$ because every node is visited precisely once. However, the subsequent required sorting step runs in $O(K \log K)$ where $K$ is the quantity of siblingless nodes. In a completely skewed tree, $K \approx N$, elevating the asymptotic boundary to $O(N \log N)$.
- **Space Complexity:** $O(H)$. The recursive stack consumes dynamic memory proportional to the depth of the tree, reaching $O(N)$ for pathological skews. The result vector also consumes $O(K)$ space.
