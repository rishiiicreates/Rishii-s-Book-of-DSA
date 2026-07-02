---
type: concept
tags: [tree, binary-tree, recursion, string-matching, cpp, math]
date: 2026-06-30
---
# Subtree of Another Tree

## Problem Statement
Given two exact structural geometric binary trees bounded independently as `root` and `subRoot`, evaluate strictly mathematically whether `subRoot` exists structurally identical to any discrete, completely bounded contiguous spatial subspace contained within `root`. A valid subspace strictly encapsulates a temporal node paired identically alongside all its respective topological descendants down to terminal null bounds.

---

## Approach: Recursive Geometric Traversal and Structural Equivalence

The challenge recursively demands satisfying exactly two specific mathematical preconditions:
1. Identifying a candidate topological anchor point $U$ in `root` which mimics the absolute scalar values.
2. Triggering a synchronized continuous sequence geometry check evaluating whether $U$ and `subRoot` share absolute spatial equivalence.

Algorithm:
1. Define an explicit structural helper function `isIdentical(node1, node2)`. This mathematically dictates synchronous simultaneous DFS preorders. It returns false immediately if boundaries conflict (one topology halts early or structural values mismatch), and true if both simultaneously hit temporal null boundaries structurally seamlessly.
2. Define the localized recursive boundary evaluator `isSubtree(root, subRoot)`.
3. If `root` mathematically evaluates identically null, return false immediately (impossible geometry).
4. Evaluate continuous spatial logic: Does the anchor start perfectly bounded here `isIdentical(root, subRoot)`?
5. If negative, forcefully penetrate leftward (`isSubtree(root->left)`) OR rightward (`isSubtree(root->right)`), continuously querying geometrical overlaps.

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

// Strict Mathematical evaluation comparing Dual Topological Graphs
bool isIdentical(TreeNode* n1, TreeNode* n2) {
    // Spatial geometric terminal boundaries overlap identical states
    if (n1 == nullptr && n2 == nullptr) return true;
    
    // One boundary unilaterally fractures structurally or algebraic scalars diverge
    if (n1 == nullptr || n2 == nullptr || n1->val != n2->val) return false;
    
    // Recursive simultaneous synchronous topological penetration
    return isIdentical(n1->left, n2->left) && isIdentical(n1->right, n2->right);
}

bool isSubtree(TreeNode* root, TreeNode* subRoot) {
    // A null continuous space cannot physically encapsulate a defined sub-geometry
    if (root == nullptr) return false;
    
    // Absolute geometrical identity confirmed perfectly
    if (isIdentical(root, subRoot)) return true;
    
    // Recursively probe discrete independent subspaces globally left and right
    return isSubtree(root->left, subRoot) || isSubtree(root->right, subRoot);
}
```

> [!important]
> For highly dense geometric graphs processing absolute optimal performance demands, KMP (Knuth-Morris-Pratt) string matching structurally evaluates identical states geometrically in $\mathcal{O}(M+N)$. This entails fully serializing both trees and searching strictly for a mathematical string subset mapping in bounded linear time.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N \times M)$ worst-case structural evaluation. For every discrete spatial boundary anchored in `root` (size $N$), an identical evaluation (size $M$) mathematically triggers completely.
- **Space Complexity:** $\mathcal{O}(\max(H_N, H_M))$, defined by the explicit maximal recursion stack depths penetrated globally simultaneously traversing spatial graphs.
