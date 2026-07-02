---
type: concept
tags: [bst, cpp, math, recursion, reverse-inorder]
date: 2026-07-01
---
# Kth Largest Element in a BST

## Problem Statement
Given the topological root of a Binary Search Tree (BST) and an integer parameter $K$, compute the $K$-th absolute largest scalar contained within the geometric graph.

---

## Approach: Reverse In-Order Monotonic Sequence

While the $K$-th smallest mathematically aligns with an $(N - K + 1)$-th largest node, calculating absolute $N$ requires a full $O(N)$ structural traversal.
Instead, we execute the mathematical dual of an In-Order traversal: a **Reverse In-Order Traversal (Right, Root, Left)**.
This geometrically visits all nodes in strictly monotonic **descending** scalar order.
1. Recursively plunge to the absolute rightmost geometry (the global maximum scalar).
2. As the stack retracts, mutate a global `count++`.
3. When `count == K`, lock the `ans` state and globally collapse the DFS sequence.

---

## Code Implementation

```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

void reverseInorder(TreeNode* root, int k, int& count, int& ans) {
    // Base Case & Short Circuit Bounds
    if (root == nullptr || count >= k) return;
    
    // Exhaust absolute maximum geometric scalars first
    reverseInorder(root->right, k, count, ans);
    
    // Extrema scalar evaluation
    count++;
    if (count == k) {
        ans = root->val;
        return; // Collapse sequence geometry
    }
    
    // Descend into smaller scalar structures
    reverseInorder(root->left, k, count, ans);
}

int kthLargest(TreeNode* root, int k) {
    int count = 0;
    int ans = -1;
    reverseInorder(root, k, count, ans);
    return ans;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(H + K)$ amortized constraint. Plunging to the rightmost leaf is bounded by $O(H)$, and extracting the required sequence geometry costs $O(K)$.
- **Space Complexity:** $O(H)$ bounding the recursive depth mapping context.

> [!important]
> **Symmetry of Duals:** This isolates the identical algorithmic and mathematical logic of `Kth Smallest`, merely applying a geometric phase shift to traverse Right edges prior to Left edges.
