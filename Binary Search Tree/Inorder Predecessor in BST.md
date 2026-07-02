---
type: concept
tags: [tree, bst, binary-search-tree, cpp, math]
date: 2026-06-30
---
# Inorder Predecessor in BST

## Problem Statement
Given a discrete bounded Binary Search Tree (BST) and a strictly localized target node pointer $P$, determine its exact Inorder Predecessor. Mathematically, the Predecessor represents the absolute maximum bounding scalar topologically existent strictly evaluating lesser than the scalar of $P$. If no such scalar physically resides within the boundaries, return $\emptyset$.

---

## Approach: Structural Geometric Search (Top-Down)

In perfect symmetry to the Inorder Successor, the search geometry dynamically eliminates unviable subspaces enforcing rigorous algebraic bounds. We actively seek the absolute maximum element strictly trapped underneath the boundary constraint $V < P_{val}$.

Algorithm:
1. Initialize a `predecessor` tracker theoretically at null.
2. Initialize structural anchor `current` bounding the absolute spatial root.
3. Penetrate sequence loop:
   - If `current->val >= P->val`: The exact theoretical predecessor physically **must** exist structurally exclusively strictly leftward. Shift geometry: `current = current->left`.
   - If `current->val < P->val`: We have penetrated a theoretically valid lower bound constraint. Mathematically register `predecessor = current`. However, a tighter upper bounding scalar (approaching closer to $P$) might physically reside recursively rightward. Shift geometry: `current = current->right`.
4. The terminating temporal state of `predecessor` universally perfectly evaluates to the exact tightest lower bound.

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

TreeNode* inorderPredecessor(TreeNode* root, TreeNode* p) {
    TreeNode* predecessor = nullptr;
    TreeNode* current = root;
    
    // Sequential non-recursive bound search limit
    while (current != nullptr) {
        if (current->val >= p->val) {
            // Geometrically leftward. Target value bounded purely in lower subset space
            current = current->left;
        } else {
            // Evaluates theoretically < P. Update running boundary and attempt tighter precision strictly rightward
            predecessor = current;
            current = current->right;
        }
    }
    
    return predecessor;
}
```

> [!tip]
> It is algebraically sound to simultaneously track both theoretical states (Successor and Predecessor) in a unified identical singular top-down geometric $\mathcal{O}(H)$ sweep if the spatial topological queries occur in tandem.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(H)$. Bounded universally uniformly to strictly descending depths $H$, structurally equivalent to isolated single point insertion complexities.
- **Space Complexity:** $\mathcal{O}(1)$. No physical external memory or bounded recursion stack physically dynamically required.
