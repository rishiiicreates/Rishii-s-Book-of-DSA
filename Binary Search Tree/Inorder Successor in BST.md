---
type: concept
tags: [tree, bst, binary-search-tree, cpp, math]
date: 2026-06-30
---
# Inorder Successor in BST

## Problem Statement
Given a discrete bounded Binary Search Tree (BST) and a strictly localized target node pointer $P$, determine its exact Inorder Successor. Mathematically, the Successor represents the absolute minimum bounding scalar topologically existent strictly evaluating greater than the scalar of $P$. If no such scalar physically resides within the boundaries, return $\emptyset$.

---

## Approach: Structural Geometric Search (Top-Down)

Unlike standard geometric binary trees demanding complete unweighted DFS penetration ($\mathcal{O}(N)$), the BST invariant directly dictates spatial causal branching bounded exclusively within $\mathcal{O}(H)$.
We do not recursively enumerate sequences; instead, we sequentially isolate topological search spaces eliminating redundant subtrees geometrically.

Algorithm:
1. Initialize a `successor` tracker theoretically at null.
2. Initialize structural anchor `current` bounding the absolute spatial root.
3. Penetrate sequence loop:
   - If `current->val <= P->val`: The exact theoretical successor physically **must** exist structurally exclusively strictly rightward. Shift geometry: `current = current->right`.
   - If `current->val > P->val`: We have penetrated a theoretically valid upper bound constraint. Mathematically register `successor = current`. However, a tighter lower bounding scalar might physically reside recursively leftward. Shift geometry: `current = current->left`.
4. The terminating temporal state of `successor` universally perfectly evaluates to the exact tightest upper bound.

---

## Code Implementation

```cpp
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
    TreeNode* successor = nullptr;
    TreeNode* current = root;
    
    // Sequential non-recursive bound search limit
    while (current != nullptr) {
        if (current->val <= p->val) {
            // Geometrically rightward. Target value bounded purely in upper subset space
            current = current->right;
        } else {
            // Evaluates theoretically > P. Update running boundary and attempt tighter precision strictly leftward
            successor = current;
            current = current->left;
        }
    }
    
    return successor;
}
```

> [!important]
> For topologies encompassing explicit `parent` backward pointers per node, if $P$ directly features a right child topology, the exact successor physically evaluates precisely as the absolute structural leftmost geometry nested beneath $P_R$. The algorithm natively inherently accommodates this structurally without edge deviations.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(H)$. Bounded universally uniformly to strictly descending depths $H$, completely bypassing $\mathcal{O}(N)$ planar iteration.
- **Space Complexity:** $\mathcal{O}(1)$. Mathematical unweighted variables dictate purely spatial constants devoid of dynamic memory demands.
