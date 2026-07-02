---
type: concept
tags: [binary-search-tree, recursion, insertion, cpp]
date: 2026-06-30
---
# Insert into a Binary Search Tree

## Problem Statement
Given the root of a [[Binary Search Tree]] and a scalar value `val`, mathematically insert the new value into the tree such that the strict topological properties of the BST are perfectly preserved. Assume all values are strictly unique.

---

## Approach: Recursive Descent and Leaf Attachment

Insertion fundamentally operates exactly like [[Search in BST]]. We trace the logical path where the value *should* exist. Since the value is guaranteed to not be in the tree, our search will mathematically terminate at a `nullptr` boundary (a missing child of a leaf node). This exact boundary is unequivocally the correct topological position for insertion.

1. **Base Constraint:** If the current `node` evaluates to `nullptr`, instantiate a new node with `val` and return its memory address.
2. **Recursive Pathing:** 
   - If `val < node->val`, recursively assign `node->left = insertIntoBST(node->left, val)`.
   - If `val > node->val`, recursively assign `node->right = insertIntoBST(node->right, val)`.
3. **Return Subtree Root:** Return the current `node` to maintain link continuity up the recursive call stack.

> [!important]
> By re-assigning `node->left` or `node->right` to the result of the recursive call, we elegantly abstract away the complex pointer wiring required when attaching the new node. If the subtree already exists, it simply returns its own address, causing a harmless self-assignment. 

---

## Code Implementation (Recursive)

```cpp
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

TreeNode* insertIntoBST(TreeNode* root, int val) {
    // Spatial vacancy discovered; attach new node
    if (root == nullptr) {
        return new TreeNode(val);
    }
    
    // Navigate structural bounds
    if (val < root->val) {
        root->left = insertIntoBST(root->left, val);
    } else if (val > root->val) {
        root->right = insertIntoBST(root->right, val);
    }
    
    // Node already exists handling (ignored per constraint of unique keys)
    // Return unmodified root to sustain structural integrity
    return root;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(H)$, where $H$ is the topological height of the tree. Insertion requires exactly one traversal from the root to a leaf boundary. Bounded by $O(\log N)$ in balanced trees and $O(N)$ in degenerate topologies.
- **Space Complexity:** $O(H)$ auxiliary space allocated for the recursive call stack. This can be optimized to $O(1)$ by adopting an iterative `while` loop implementation that explicitly wires parent pointers.
