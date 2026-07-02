---
type: concept
tags: [bst, cpp, math, recursion, inorder]
date: 2026-07-01
---
# Kth Smallest Element in a BST

## Problem Statement
Given the geometric root of a Binary Search Tree (BST) and an integer constant $K$, mathematically locate the $K$-th smallest scalar sequence value present in the entire topological structure (1-indexed).

---

## Approach: In-Order Monotonic Extraction

The defining mathematical theorem of a BST is that an **In-Order Traversal (Left, Root, Right)** geometrically visits nodes in strictly monotonic ascending scalar order.
To discover the $K$-th smallest scalar:
1. We construct a topological In-Order DFS recursive descent.
2. We inject a global state tracker `count = 0` and an algebraic target `ans = -1`.
3. The absolute moment the traversal evaluates the current vertex ($T$), we mutate `count++`.
4. If `count == K`, we have successfully mapped the target. Log `ans = T.val` and propagate a short-circuit return to immediately collapse the remaining execution stack.

---

## Code Implementation

```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

void inorder(TreeNode* root, int k, int& count, int& ans) {
    // Base Case & Short Circuit
    if (root == nullptr || count >= k) return;
    
    // Geometrically exhaust left topology (smallest scalars)
    inorder(root->left, k, count, ans);
    
    // Mathematical vertex evaluation
    count++;
    if (count == k) {
        ans = root->val;
        return; // Trigger global stack collapse
    }
    
    // Geometrically exhaust right topology (larger scalars)
    inorder(root->right, k, count, ans);
}

int kthSmallest(TreeNode* root, int k) {
    int count = 0;
    int ans = -1;
    inorder(root, k, count, ans);
    return ans;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(H + K)$ amortized extraction. To find the minimum vertex bounds $O(H)$, and traversing $K$ nodes bounds $O(K)$. The worst-case is strictly $O(N)$ when $K \approx N$.
- **Space Complexity:** $O(H)$ bounded by the recursive DFS stack limit.

> [!tip]
> **Morris Traversal Optimization:** For absolute $O(1)$ spatial perfection, a Morris In-Order Traversal can be constructed, executing topological thread deformation to achieve the sequence without call-stack overhead, though the runtime coefficient increases slightly due to threading manipulation.
