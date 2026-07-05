# Maximum Path Sum in Binary Tree

## Problem Statement
- given a non-empty binary tree where nodes contain integer scalars (potentially negative), mathematically evaluate the maximum topological path sum. A valid geometric path is defined as any continuous sequence of connected nodes from some starting node to any node in the tree along the parent-child connections. A node can structurally appear exactly once in the sequence, and the path is not strictly mandated to traverse through the absolute root.


## Approach: Bottom-Up Depth First Search (DFS)

- the structural complexity arises because the maximum path intersecting a given geometric node $U$ might "fork", incorporating both $U$'s left subtree path and right subtree path. However, a path evaluating *higher* up the tree can only geometrically utilize *at most one* of $U$'s children to maintain continuity.

- we theoretically decouple the evaluation using a recursive Postorder DFS invariant:
- for any node $U$, recursively calculate the maximum unbranched path sum extending downwards into its left subspace: `leftMax = max(0, DFS(U->left))`. We strictly clip negative sums to $0$ as incorporating a net-negative geometric branch mathematically degrades the global optimum.
- symmetrically calculate `rightMax = max(0, DFS(U->right))`.
- the absolute local maximum path structurally bridging through $U$ evaluates to: `local_max = U->val + leftMax + rightMax`. We continually update the global mathematical maximum with this value.
- for $U$ to contribute to a path controlled by its topological parent, it can only geometrically offer a single linear continuous line. Thus, it structurally returns: `U->val + max(leftMax, rightMax)`.


## Code Implementation

```cpp
#include <algorithm>
#include <climits>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int maxPathSumHelper(TreeNode* node, int& global_max) {
    if (node == nullptr) return 0; // Null subspace evaluates to 0
    
    // Evaluate unilateral spatial contributions, clipping mathematical deficits
    int left_max = max(0, maxPathSumHelper(node->left, global_max));
    int right_max = max(0, maxPathSumHelper(node->right, global_max));
    
    // Evaluate complete geometric path arching through the current node
    int current_arch = node->val + left_max + right_max;
    global_max = max(global_max, current_arch);
    
    // Return strictly one unbranched continuous line to the structural parent
    return node->val + max(left_max, right_max);
}

int maxPathSum(TreeNode* root) {
    int global_max = INT_MIN; // Global optimum bound
    maxPathSumHelper(root, global_max);
    return global_max;
}
```

> [!warning]
> The global optimum `global_max` must be initialized strictly to `INT_MIN`. If all geometric nodes in the tree contain negative scalars, the mathematically correct maximum path is simply the single node bearing the absolute smallest negative value. Initializing to $0$ incorrectly assumes an empty null-path is a valid topology.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. Every distinct node is penetrated and mathematically evaluated exclusively once via the Postorder traversal.
- **space Complexity:** $\mathcal{O}(H)$, dictated strictly by the recursion depth bounding the System Call Stack. $H = N$ dynamically in heavily skewed configurations.

NEXT: [[Index]]
