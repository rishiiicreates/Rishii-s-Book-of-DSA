# Validate Binary Search Tree

## Problem Statement
- given a generic topological Binary Tree, strictly prove whether it conforms to the axiomatic mathematical definition of a valid Binary Search Tree (BST).
- the mathematical invariant requires:
- every node in the left subtree $T_L$ is strictly less than $T$.
- every node in the right subtree $T_R$ is strictly greater than $T$.
- subtrees $T_L$ and $T_R$ must recursively satisfy the invariant.


## Approach: Recursive Absolute Geometric Bounds

- a naive verification mapping only a node to its immediate children is mathematically catastrophic, as it fails to enforce the global boundary transitive constraints.
- instead, we utilize **Recursive Absolute Extrema Bounds**.
- every structural node must geographically reside within an explicit algebraic interval $(L, R)$.
- the global geometric root is unconstrained: $(-\infty, \infty)$.
- when descending to a left child $T_L$, the strict upper boundary condenses to the parent's scalar: $(L, \text{Parent})$.
- when descending to a right child $T_R$, the strict lower boundary expands to the parent's scalar: $(\text{Parent}, R)$.
- if any vertex intrinsically violates $L < V.\text{val} < R$, the entire topology is irrevocably invalid.


## Code Implementation

```cpp
#include <climits>

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

bool isValidBSTUtil(TreeNode* node, long min_val, long max_val) {
    // Base Identity: Empty graph is a trivially valid BST
    if (node == nullptr) return true;
    
    // Algebraic Violation Verification
    if (node->val <= min_val || node->val >= max_val) {
        return false;
    }
    
    // Inductive Structural Propagation
    return isValidBSTUtil(node->left, min_val, node->val) && 
           isValidBSTUtil(node->right, node->val, max_val);
}

bool isValidBST(TreeNode* root) {
    // 64-bit integer limits required to bound extreme 32-bit test vectors
    return isValidBSTUtil(root, LONG_MIN, LONG_MAX);
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ strict linear traversal. The algorithm evaluates every structural vertex exactly once, propagating $O(1)$ boundary state.
- **space Complexity:** $O(H)$ spatial bounds corresponding to the DFS call-stack recursion depth.

> [!warning]
> **In-Order Trap:** While an In-Order traversal of a valid BST mathematically produces a strictly ascending monotonic sequence, relying on sequence array extraction requires $O(N)$ space. Storing only the `prev` scalar pointer solves space issues but complicates short-circuit propagation. Boundary constraints are algebraically superior.

NEXT: [[Index]]
