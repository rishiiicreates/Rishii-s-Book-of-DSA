# Kth Largest Element in a BST

## Problem Statement
- given the topological root of a Binary Search Tree (BST) and an integer parameter $K$, compute the $K$-th absolute largest scalar contained within the geometric graph.


## Approach: Reverse In-Order Monotonic Sequence

- while the $K$-th smallest mathematically aligns with an $(N - K + 1)$-th largest node, calculating absolute $N$ requires a full $O(N)$ structural traversal.
- instead, we execute the mathematical dual of an In-Order traversal: a **Reverse In-Order Traversal (Right, Root, Left)**.
- this geometrically visits all nodes in strictly monotonic **descending** scalar order.
- recursively plunge to the absolute rightmost geometry (the global maximum scalar).
- as the stack retracts, mutate a global `count++`.
- when `count == K`, lock the `ans` state and globally collapse the DFS sequence.


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


## Complexity Analysis
- **time Complexity:** $O(H + K)$ amortized constraint. Plunging to the rightmost leaf is bounded by $O(H)$, and extracting the required sequence geometry costs $O(K)$.
- **space Complexity:** $O(H)$ bounding the recursive depth mapping context.

> [!important]
> **Symmetry of Duals:** This isolates the identical algorithmic and mathematical logic of `Kth Smallest`, merely applying a geometric phase shift to traverse Right edges prior to Left edges.

NEXT: [[Index]]
