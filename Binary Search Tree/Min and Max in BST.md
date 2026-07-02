---
type: concept
tags: [binary-search-tree, tree-traversal, cpp]
date: 2026-06-30
---
# Minimum and Maximum in BST

## Problem Statement
Given the root of a [[Binary Search Tree]], mathematically extract the absolute minimum and the absolute maximum scalar values contained within the entire structure. 

---

## Approach: Directed Edge Traversal

The strict topological invariants of the BST dictate that:
- Every element in a node's left subtree is strictly less than the node itself. Thus, traversing continuously down the left edges minimizes the value monotonically.
- Every element in a node's right subtree is strictly greater than the node itself. Thus, traversing continuously down the right edges maximizes the value monotonically.

1. **Minimum Computation:**
   - Initialize a pointer at `root`.
   - While `pointer->left != nullptr`, advance `pointer = pointer->left`.
   - The loop terminates at the node with no left child. This node's value is unconditionally the global minimum.
2. **Maximum Computation:**
   - Initialize a pointer at `root`.
   - While `pointer->right != nullptr`, advance `pointer = pointer->right`.
   - The loop terminates at the node with no right child. This node's value is unconditionally the global maximum.

> [!tip]
> Because these algorithms rely exclusively on a unilateral topological descent (never branching or backtracking), they represent the theoretical lower limit of computational effort required to locate extrema in sorted linked structures.

---

## Code Implementation

```cpp
#include <climits>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int findMin(TreeNode* root) {
    if (root == nullptr) return INT_MAX; // Mathematical sentinel for empty tree
    
    TreeNode* current = root;
    // Monotonic descent to absolute minimum
    while (current->left != nullptr) {
        current = current->left;
    }
    
    return current->val;
}

int findMax(TreeNode* root) {
    if (root == nullptr) return INT_MIN; // Mathematical sentinel for empty tree
    
    TreeNode* current = root;
    // Monotonic descent to absolute maximum
    while (current->right != nullptr) {
        current = current->right;
    }
    
    return current->val;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(H)$, where $H$ is the height of the tree. The traversal descends exactly one path from the root to a leaf boundary. 
- **Space Complexity:** $O(1)$. We allocate a single scalar pointer variable to traverse the path iteratively, circumventing recursive call stack penalties entirely.
