---
type: concept
tags: [binary-search-tree, recursion, cpp]
date: 2026-06-30
---
# Search in a Binary Search Tree

## Problem Statement
Given the root of a [[Binary Search Tree]] and a scalar value `key`, mathematically determine if a node with `val == key` exists within the tree. If it exists, return a pointer to the node; otherwise, return `nullptr`.

---

## Approach: Directed Binary Traversal

Because of the strict invariant governing the BST ($L_{key} < N_{key} < R_{key}$), we do not need to traverse the entire topological structure. At any given node, we can logically eliminate exactly half of the remaining sub-branches (assuming a balanced tree).

1. **Base Case:** If the current `node` is `nullptr` or if `node->val == key`, return the `node`.
2. **Partition Evaluation:** 
   - If `key < node->val`, the target mathematically *must* exist within the left subtree. Recurse or iterate to `node->left`.
   - If `key > node->val`, the target mathematically *must* exist within the right subtree. Recurse or iterate to `node->right`.

> [!tip]
> Because tail recursion directly translates into a trivial loop, the iterative approach is widely preferred to eliminate the $O(H)$ recursion call stack overhead, achieving absolute $O(1)$ auxiliary space.

---

## Code Implementation (Iterative - Optimal)

```cpp
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

TreeNode* searchBST(TreeNode* root, int val) {
    TreeNode* current = root;
    
    // Traverse downwards guided by BST invariants
    while (current != nullptr) {
        if (current->val == val) {
            return current;
        } else if (val < current->val) {
            // Target is definitively in the left partition
            current = current->left;
        } else {
            // Target is definitively in the right partition
            current = current->right;
        }
    }
    
    // Path exhausted, value not in set
    return nullptr;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(H)$, where $H$ is the topological height of the tree. For a well-balanced tree, this is $O(\log N)$. For a fully degenerate tree, it degrades to $O(N)$.
- **Space Complexity:** $O(1)$ auxiliary space using the iterative method. A recursive method would incur $O(H)$ space overhead corresponding to the call stack depth.
