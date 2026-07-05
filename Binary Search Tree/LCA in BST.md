# Lowest Common Ancestor in a Binary Search Tree

## Problem Statement
- given a Binary Search Tree topology and two target scalar nodes $P$ and $Q$, mathematically isolate their Lowest Common Ancestor (LCA). The LCA is the geometrically deepest node in the topology that structurally encompasses both $P$ and $Q$ as descendants.


## Approach: Algebraic Splitting Bisection

- unlike a generic Binary Tree which requires post-order boolean bound propagation, a BST provides mathematical isolation via its inherent spatial sorting property.
- for any current geometric vertex $V$:
- if both $P$ and $Q$ are strictly smaller than $V$ ($P < V \land Q < V$), the LCA MUST geometrically reside entirely within the left disjoint subset $T_L$.
- if both $P$ and $Q$ are strictly greater than $V$ ($P > V \land Q > V$), the LCA MUST geometrically reside entirely within the right disjoint subset $T_R$.
- **bisection Theorem:** If neither of the above conditions holds, the scalars $P$ and $Q$ have geometrically bifurcated (one is smaller, one is larger, or one is identically $V$). The absolute first node where this scalar bifurcation occurs is mathematically guaranteed to be the Lowest Common Ancestor.


## Code Implementation

```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    // Base Verification
    if (root == nullptr) return nullptr;
    
    // Iterative Monotonic Descent
    while (root != nullptr) {
        int curr = root->val;
        
        // Both scalars algebraically exceed root
        if (curr < p->val && curr < q->val) {
            root = root->right;
        }
        // Both scalars algebraically inferior to root
        else if (curr > p->val && curr > q->val) {
            root = root->left;
        }
        // Scalar Bifurcation Point Reached
        else {
            return root;
        }
    }
    
    return nullptr;
}
```


## Complexity Analysis
- **time Complexity:** $O(H)$ expected traversal mapped to the height of the BST. We mathematically bypass irrelevant subtrees entirely, executing a single downward vector search.
- **space Complexity:** $O(1)$ spatial limit. The iterative nature isolates state mapping to a singular sliding pointer.

> [!important]
> **DFS Obsolescence:** Running a standard binary tree LCA algorithm $O(N)$ on a BST is mathematically sub-optimal. The BST topology guarantees the bifurcation vertex without needing topological bottom-up bound propagation.

NEXT: [[Index]]
