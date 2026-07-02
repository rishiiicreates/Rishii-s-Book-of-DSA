---
type: concept
tags: [tree, binary-tree, dfs, dynamic-programming, cpp, math]
date: 2026-06-30
---
# Longest Path with Maximal Sum (Root to Leaf)

## Problem Statement
Given a discrete binary tree, mathematically determine the maximal sequence sum strictly defined on the topologically *longest* contiguous path bridging from the absolute root exactly terminating at a leaf node. If multiple disjoint paths structurally share the exact maximal length, evaluate the one bearing the absolute maximum algebraic sum.

---

## Approach: Depth-Sum Isomorphic Evaluation

This challenge demands prioritizing two distinct mathematical variables:
1. Geometric Length (Depth $D$) - Strictly the primary structural constraint.
2. Algebraic Sum (Sum $S$) - The secondary tie-breaking constraint.

We construct a DFS Postorder or Preorder structure mathematically transmitting these dual states sequentially downwards, maintaining a global optimum strictly evaluated via structural tuple comparison.

Algorithm:
1. Construct a rigorous DFS invariant tracking temporal state: `(currentNode, currentDepth, currentSum)`.
2. Upon evaluating an absolute leaf node (both bounds are strictly `nullptr`), execute an algebraic update against the global optimum:
   - If `currentDepth > maxDepth`, aggressively overwrite both states.
   - If `currentDepth == maxDepth`, conditionally overwrite if `currentSum > maxSum`.
3. Penetrate dynamically into left and right subspaces, incrementing structural depth and algebraic sum.

---

## Code Implementation

```cpp
#include <climits>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

void solveLongestMaxSum(TreeNode* node, int depth, int sum, int& max_depth, int& max_sum) {
    if (node == nullptr) return;
    
    // Accumulate structural and scalar progression
    sum += node->val;
    
    // Geometric leaf node boundary detection
    if (node->left == nullptr && node->right == nullptr) {
        if (depth > max_depth) {
            max_depth = depth;
            max_sum = sum;
        } else if (depth == max_depth) {
            if (sum > max_sum) {
                max_sum = sum;
            }
        }
        return; // Terminal boundary structurally met
    }
    
    // Recursively penetrate spatial subspaces
    solveLongestMaxSum(node->left, depth + 1, sum, max_depth, max_sum);
    solveLongestMaxSum(node->right, depth + 1, sum, max_depth, max_sum);
}

int longestMaxSum(TreeNode* root) {
    if (!root) return 0;
    
    int max_depth = 0;
    int max_sum = INT_MIN;
    
    // Initial root exists at structural depth 1
    solveLongestMaxSum(root, 1, 0, max_depth, max_sum);
    
    return max_sum;
}
```

> [!tip]
> Structuring `max_sum` as `INT_MIN` theoretically defends against negative scalars dominating the tree, similar to all rigid path-sum formulations.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. The tree geometrically processes each individual structural element exactly once.
- **Space Complexity:** $\mathcal{O}(H)$, dictated by recursive call frames tracking deep temporal paths bounded by the structure's physical height.
