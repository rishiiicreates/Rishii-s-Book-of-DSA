# Ceil in a Binary Search Tree

## Problem Statement
- given the root of a [[Binary Search Tree]] and a scalar value `key`, mathematically compute the **Ceil** of the `key`.

- *definition:* The Ceil of `key` is defined as the absolute minimum integer $C$ present in the BST such that $C \ge \text{key}$. If no such value mathematically exists (i.e., all elements in the tree are strictly less than `key`), return a sentinel value such as $-1$.


## Approach: Guided Iterative Search

- we exploit the standard BST partitioning invariants to narrow the search space continuously, storing the "best observed candidate" as we traverse.

- **state Tracking:** Initialize a scalar `ceil_val = -1`.
- **iterative Traversal:** Begin at the `root`.
- **partition Logic:**
   - if `node->val == key`, we have found a perfect match. A value acts as its own mathematical ceiling. Return `node->val`.
   - if `node->val < key`, the current node and its entire left subtree are mathematically obsolete, as they are all strictly less than `key`. We must search for larger elements. Advance `node = node->right`.
   - if `node->val > key`, the current node is a *valid candidate* for the ceil. We record `ceil_val = node->val`. However, there might exist a tighter (smaller) valid bound. We must search for smaller elements that still exceed `key`. Advance `node = node->left`.
- **termination:** When `node` evaluates to `nullptr`, return the final recorded `ceil_val`.

> [!important]
> The topological dual to this problem is **Floor in a BST** (the maximum integer $F \le \text{key}$). The logic is perfectly mirrored: if `node->val < key`, record `floor_val = node->val` and traverse right; if `node->val > key`, traverse left.


## Code Implementation

```cpp
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int findCeil(TreeNode* root, int key) {
    int ceil_val = -1; // Sentinel for non-existence
    TreeNode* current = root;
    
    while (current != nullptr) {
        if (current->val == key) {
            // Absolute optimal bound achieved
            return current->val;
        }
        
        if (current->val < key) {
            // Current node is mathematically inferior to key
            current = current->right;
        } else {
            // Current node mathematically bounds key from above
            // Record state and attempt to minimize the bound
            ceil_val = current->val;
            current = current->left;
        }
    }
    
    return ceil_val;
}
```


## Complexity Analysis
- **time Complexity:** $O(H)$, where $H$ is the topological height of the tree. The algorithm traces a continuous monotonic path from the root to a leaf boundary, examining at most $H$ nodes.
- **space Complexity:** $O(1)$. Implemented iteratively, the algorithm requires no auxiliary dynamic memory beyond basic scalar variables.

NEXT: [[Index]]
