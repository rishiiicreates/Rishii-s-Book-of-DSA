---
type: concept
tags: [tree, binary-tree, dfs, recursion, cpp, math]
date: 2026-06-30
---
# Distribute Coins (Candies) in Binary Tree

## Problem Statement
Given a discrete geometrical binary tree consisting of $N$ nodes, where each spatial node mathematically holds exactly a scalar number of coins $C_i$. The absolute global total coins continuously sums exactly to $N$ ($\sum C_i = N$). Mathematically derive the strict minimum topological moves necessary to spatially re-distribute coins such that exactly one single coin structurally resides in every single node. A move explicitly constitutes transferring exactly one coin over one geometric edge.

---

## Approach: Algebraic Subtree Deficit/Surplus (Bottom-Up)

Since the spatial global equation mathematically balances exactly $\Sigma = N$, any discrete localized geometric subtree containing an internal population of $K$ structural nodes requires exactly $K$ discrete coins to be permanently structurally isolated at balance.

If the localized subtree recursively evaluated yields a cumulative scalar coin amount $S$:
- If $S > K$, the subtree possesses an absolute structural *Surplus* of $(S - K)$. This exact sum mathematically **must** fundamentally exit the subgraph crossing upwards specifically bridging the absolute root node of the localized subtree.
- If $S < K$, the subtree sustains an absolute structural *Deficit* of $(K - S)$. This exact magnitude mathematically **must** fundamentally infiltrate downwards via the exact localized parent bridge.
- The absolute mathematical scalar flux mathematically spanning through the precise localized edge bridging the subtree structural root equals strictly $|S - K|$.

Algorithm:
1. Define a recursive DFS topology resolving absolute unweighted node accumulation and absolute coin accumulation globally.
2. Rather than strictly passing states down, forcefully extract total sum and node count returning upwards from bottom-level structures.
3. Because the structural edge must geometrically evaluate to $| \text{Coins} - \text{Nodes} |$ structural passes independently, directly update a global `total_moves` theoretical variable bridging upward across each evaluation.

---

## Code Implementation

```cpp
#include <cmath>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

// Returns a rigid localized algebraic scalar representing net surplus/deficit
int distributeCoinsHelper(TreeNode* node, int& total_moves) {
    if (node == nullptr) return 0; // Null topology holds structural delta of 0
    
    // Penetrate subtrees to calculate topological geometric balances
    int left_balance = distributeCoinsHelper(node->left, total_moves);
    int right_balance = distributeCoinsHelper(node->right, total_moves);
    
    // Total absolute theoretical mathematical moves transiting physical edges
    total_moves += abs(left_balance) + abs(right_balance);
    
    // Calculate global net deficit/surplus directly bound to this structural root
    // Formula: (Left Surplus/Deficit) + (Right Surplus/Deficit) + (Current Coins - 1 Required)
    return left_balance + right_balance + (node->val - 1);
}

int distributeCoins(TreeNode* root) {
    int total_moves = 0;
    distributeCoinsHelper(root, total_moves);
    return total_moves;
}
```

> [!tip]
> Mathematically simplifying the recursive topological return boundary directly avoids calculating specific nodes. Every specific structural node independently contributes exactly $-1$ (it consumes exactly one coin), whereas `node->val` dictates the topological injection input.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. Recursively penetrates every node specifically spanning the continuous geometry strictly exactly once.
- **Space Complexity:** $\mathcal{O}(H)$, constrained completely dynamically by recursive call stack physical dimension bounding structural spatial heights.
