# Lowest Common Ancestor (LCA)

## Problem Statement
- given a discrete geometric binary tree alongside two exact structural pointers targeting nodes $p$ and $q$, mathematically compute their Lowest Common Ancestor (LCA). The LCA is rigorously defined as the absolute lowest distinct node $T$ bounding both $p$ and $q$ strictly within its topological descendant subspace. A node is structurally permitted to act as an ancestor directly mapping to itself.


## Approach: Bottom-Up Depth First Search (DFS)

- since binary structures completely lack spatial back-pointers to parents, structural isolation mathematically mandates a purely bottom-up Postorder propagation.
- we must dynamically interrogate the geometry: "Does node $X$ reside recursively bounding the left subspace, or the right subspace?"

- algorithm:
- base Boundary: If the recursively tracked `current` node converges exactly on bounds `p`, `q`, or structural null $\emptyset$, return `current` itself.
- geometrically penetrate the left subspace: `left_result = DFS(node->left)`.
- symmetrically penetrate the right subspace: `right_result = DFS(node->right)`.
- **resolution [[Matrix]]**:
   - if *both* `left_result` and `right_result` explicitly yield non-null states, it theoretically guarantees that $p$ and $q$ reside structurally bifurcated on fundamentally opposite subtrees of `current`. Therefore, `current` intrinsically resolves to the absolute LCA.
   - if *exactly one* result evaluates non-null, it implies both $p$ and $q$ are structurally bottlenecked strictly downstream within that precise identical topological direction. The absolute LCA resolves upward directly tracking the non-null returning state.
   - if both bounds return absolute null, neither node exists beneath the geometric sub-structure.


## Code Implementation

```cpp
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    // 1. Geometric Boundary and Direct Hit evaluation
    if (root == nullptr || root == p || root == q) {
        return root;
    }
    
    // 2 & 3. Deep temporal subspace penetration (Bottom-up tracking)
    TreeNode* left_lca = lowestCommonAncestor(root->left, p, q);
    TreeNode* right_lca = lowestCommonAncestor(root->right, p, q);
    
    // 4. Absolute Structural Resolution
    
    // Bifurcated geometry: p and q span completely opposite spatial subspaces
    if (left_lca != nullptr && right_lca != nullptr) {
        return root;
    }
    
    // Unilateral linear downstream geometry
    return (left_lca != nullptr) ? left_lca : right_lca;
}
```

> [!warning]
> This mathematically elegant invariant intrinsically strictly presumes both targets $p$ and $q$ physically structurally reside within the bounded tree. If one target evaluates completely missing from the tree geometry, the algorithm mathematically fails, falsely returning the singular localized existent node.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$ worst-case. The structure physically penetrates unpruned spatial geometry continuously searching for exact structural hits.
- **space Complexity:** $\mathcal{O}(H)$, governed completely by the absolute height boundaries restricting the recursive Call Stack dimensions.

NEXT: [[Index]]
