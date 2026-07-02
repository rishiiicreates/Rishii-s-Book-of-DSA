---
type: concept
tags: [tree, bst, binary-search-tree, dfs, recursion, cpp, math]
date: 2026-06-30
---
# Convert BST to Greater Sum Tree

## Problem Statement
Given a discrete Binary Search Tree (BST), mathematically transform every geometric node such that its new internal scalar value dictates the summation of its original value strictly superposed with all original topological scalar values physically residing within the tree that strictly evaluate as greater than it mathematically ($> V_{curr}$).

---

## Approach: Reverse Inorder Geometric Accumulation

The defining structural invariant of a standard BST dictates that a standard topological Inorder Traversal (Left $\rightarrow$ Root $\rightarrow$ Right) inherently strictly projects a monotonically increasing geometric sequence.
Conversely, a Reverse Inorder Traversal (Right $\rightarrow$ Root $\rightarrow$ Left) mathematically generates a strictly descending array.

By establishing an external global running sum $S$ initialized strictly at $S=0$:
1. Recursively penetrate the rightward subspace strictly (approaching absolute maximums).
2. As geometry returns towards roots, algebraically accumulate: $S = S + V_{original}$.
3. Topologically mutate the current node's internal state explicitly to the evaluated state of $S$.
4. Pass the temporal causality $S$ uniformly penetrating down the leftward descending sequence.

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

// Global state boundary carried explicitly across topology
void convertToGreaterSum(TreeNode* node, int& running_sum) {
    if (node == nullptr) return; // Topological void boundary
    
    // 1. Unilateral maximum rightward sequence penetration
    convertToGreaterSum(node->right, running_sum);
    
    // 2 & 3. Algebraic accumulation and strict geometric mutation
    running_sum += node->val;
    node->val = running_sum;
    
    // 4. Continued downward temporal leftward projection
    convertToGreaterSum(node->left, running_sum);
}

TreeNode* bstToGst(TreeNode* root) {
    int running_sum = 0;
    convertToGreaterSum(root, running_sum);
    return root;
}
```

> [!tip]
> Morris Traversal algorithms mathematically permit achieving $\mathcal{O}(1)$ supplementary temporal space bounds, avoiding absolute recursive bounds $H$. However, applying standard Reverse Morris mandates temporarily mutating right-pointers (thread construction) and structurally mutating topological values before un-threading.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. Unilateral $\mathcal{O}(N)$ reverse traversal traversing discrete geometries uniquely exactly once.
- **Space Complexity:** $\mathcal{O}(H)$. Governed mathematically dynamically by call stack height limitations bound rigidly to absolute subtree depths.
