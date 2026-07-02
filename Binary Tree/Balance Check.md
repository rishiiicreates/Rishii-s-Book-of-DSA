---
type: concept
tags: [binary-tree, cpp, math, recursion, optimization]
date: 2026-07-01
---
# Check for Balanced Binary Tree

## Problem Statement
Mathematically verify if a given Binary Tree $T$ is geometrically "Height-Balanced". A tree satisfies this topology if, for *every* structural vertex $v$, the absolute difference in height between its disjoint left subtree $T_L$ and right subtree $T_R$ is strictly $\le 1$.

---

## Approach: Post-Order Bound Propagation

A naive topological algorithm mathematically evaluates the exact height of $T_L$ and $T_R$ independently for every single node. This geometric duplication forces the recurrence relation $T(N) = 2T(N/2) + O(N)$, synthesizing a catastrophic $O(N \log N)$ or worse $O(N^2)$ temporal bound.

To strictly collapse the time complexity to $O(N)$, we unify the height calculation and the geometric limit verification into a single atomic recursion.
We define a mathematical signal value, `-1`, to denote an absolute topological failure (Unbalanced state).
For any node:
1. Compute the structural height of the Left child. If it evaluates to `-1`, aggressively short-circuit and propagate `-1` up.
2. Compute the structural height of the Right child. If it evaluates to `-1`, propagate `-1`.
3. If both subtrees are geometrically valid, compute the topological absolute difference $\Delta = |H_L - H_R|$.
4. If $\Delta > 1$, trigger structural failure (`-1`). Otherwise, propagate the mathematically sound maximum height bound $1 + \max(H_L, H_R)$.

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

// Returns structural height, or -1 on bound violation
int checkHeight(TreeNode* root) {
    if (root == nullptr) return 0;
    
    int left_height = checkHeight(root->left);
    if (left_height == -1) return -1; // Short-circuit propagation
    
    int right_height = checkHeight(root->right);
    if (right_height == -1) return -1; // Short-circuit propagation
    
    // Mathematical Bound Verification
    if (abs(left_height - right_height) > 1) {
        return -1; // Trigger Failure
    }
    
    return 1 + max(left_height, right_height);
}

bool isBalanced(TreeNode* root) {
    // Isomorphism test
    return checkHeight(root) != -1;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ absolute linear scan. Every structural vertex is processed exactly once before synthesizing the unified geometric height.
- **Space Complexity:** $O(H)$ defining the execution recursion stack bound.

> [!important]
> **AVL and Red-Black Foundations:** This exact geometric height restriction (difference $\le 1$) is the foundational mathematical constraint of an AVL Tree. Verifying balance in strict $O(N)$ forms the primitive basis for all dynamic self-balancing topological structures.
