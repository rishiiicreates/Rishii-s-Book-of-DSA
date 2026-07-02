---
type: concept
tags: [binary-tree, cpp, math, recursion, bounds]
date: 2026-07-01
---
# Maximum Difference Between Node and Ancestor

## Problem Statement
Given the geometric root of a Binary Tree $T$, mathematically determine the maximum absolute scalar difference $V = |A.\text{val} - B.\text{val}|$, where $A$ and $B$ are distinct topological vertices such that $A$ is a direct structural ancestor of $B$.

---

## Approach: Recursive Absolute Extrema Propagation

Because the problem structurally mandates an Ancestor-Descendant relationship, the mathematical invariant is: for any path from root to a remote leaf, the absolute maximum scalar difference MUST occur strictly between the global path maximum value and the global path minimum value.

We execute a top-down DFS traversal. As the recursive state penetrates deeper into the geometry:
1. We propagate the absolute mathematical extrema: `current_max` and `current_min` observed *thus far* along the structural path.
2. At any generic vertex $v$, the maximum divergence involving $v$ and any valid ancestor is exactly $\max(|v.\text{val} - \text{current\_max}|, |v.\text{val} - \text{current\_min}|)$.
3. We dynamically update the mathematical bounds: $\text{new\_max} = \max(\text{current\_max}, v.\text{val})$, $\text{new\_min} = \min(\text{current\_min}, v.\text{val})$, and propagate them to disjoint subtrees $T_L$ and $T_R$.

---

## Code Implementation

```cpp
#include <algorithm>
#include <cmath>

using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int maxAncestorDiffUtil(TreeNode* node, int cur_min, int cur_max) {
    // Base Boundary Condensation
    if (node == nullptr) {
        // Return absolute path divergence
        return cur_max - cur_min;
    }
    
    // Geometrically update extrema limits
    cur_min = min(cur_min, node->val);
    cur_max = max(cur_max, node->val);
    
    // Propagate scalar bounds structurally
    int left_diff = maxAncestorDiffUtil(node->left, cur_min, cur_max);
    int right_diff = maxAncestorDiffUtil(node->right, cur_min, cur_max);
    
    return max(left_diff, right_diff);
}

int maxAncestorDiff(TreeNode* root) {
    if (!root) return 0;
    return maxAncestorDiffUtil(root, root->val, root->val);
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ strict linear evaluation. Every structural topological node is mapped exactly once, propagating constant mathematical state.
- **Space Complexity:** $O(H)$ mapping recursive depth bounds, structurally evaluating to $O(N)$ max and $O(\log N)$ perfectly balanced.

> [!warning]
> **Bottom-Up Inefficiency:** Attempting this structurally bottom-up (where a node asks for the minimum and maximum of its *entire subtree*) achieves identical $O(N)$ time, but generates severe mathematical code bloat, requiring multi-dimensional tuple returns. Top-down monotonic scalar propagation is fundamentally superior.
