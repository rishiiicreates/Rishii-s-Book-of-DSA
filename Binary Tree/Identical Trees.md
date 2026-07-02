---
type: concept
tags: [binary-tree, cpp, math, recursion]
date: 2026-07-01
---
# Check if Two Trees are Identical

## Problem Statement
Given the topological roots of two disjoint Binary Trees, $P$ and $Q$, mathematically verify if they are absolutely structurally isomorphic and if all corresponding scalar vertices hold equivalent values.

---

## Approach: Synchronous Recursive Isomorphism

Two complex geometric tree topologies are mathematically identical if and only if they satisfy the following concurrent properties recursively:
1. If both structural roots are geometrically null ($\emptyset$), they are trivially identical.
2. If exactly one root is null, the geometric topology breaks immediately.
3. If both nodes exist, their scalar integer values must be mathematically equivalent ($P.\text{val} == Q.\text{val}$).
4. Concurrently, their entire left disjoint subtrees must be identical, AND their entire right disjoint subtrees must be identical.

We execute a mathematically synchronized simultaneous DFS traversal over both trees.

---

## Code Implementation

```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

bool isSameTree(TreeNode* p, TreeNode* q) {
    // Dual Empty Base Case
    if (p == nullptr && q == nullptr) {
        return true;
    }
    
    // Asymmetric Topology Failure
    if (p == nullptr || q == nullptr) {
        return false;
    }
    
    // Scalar Equivalency Verification
    if (p->val != q->val) {
        return false;
    }
    
    // Strict Simultaneous Geometric Verification
    return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(\min(N, M))$ where $N$ and $M$ are the topological cardinalities of trees $P$ and $Q$. The algorithm mathematically terminates strictly upon observing the first topological or scalar divergence.
- **Space Complexity:** $O(\min(H_P, H_Q))$ bounding the synchronized execution call-stack depth limits.

> [!tip]
> **Serialization Equivalent:** Mathematically, checking structural isomorphism is fundamentally equivalent to independently serializing both trees (e.g., to a Post-Order string topology explicitly encoding `null` pointers) and computing a direct $O(N)$ string matching algorithm. However, synchronous traversal avoids explicit geometric memory allocation, rendering it vastly superior.
