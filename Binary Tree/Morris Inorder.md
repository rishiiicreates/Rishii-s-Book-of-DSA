---
type: concept
tags: [tree, binary-tree, traversal, cpp, math, constant-space]
date: 2026-06-30
---
# Morris Inorder Traversal

## Problem Statement
Mathematically evaluate the geometric Inorder sequence of a Binary Tree achieving strictly linear $\mathcal{O}(N)$ temporal performance whilst utilizing an absolutely constrained $\mathcal{O}(1)$ supplementary structural space, entirely avoiding both explicit Data Stacks and implicit System Call Stacks.

---

## Approach: Structural Threading (Morris Traversal)

A standard [[Inorder]] execution necessitates $\mathcal{O}(H)$ spatial memory to theoretically re-evaluate the parent after structurally resolving the left child subspace.
Morris Traversal bypasses this constraint by dynamically threading the binary graph: it mathematically restructures the read-only $\emptyset$ bounds (null pointers) of terminal leaves to explicitly point temporally backward to their Inorder successor.

Algorithm:
1. Initialize `current` at the absolute root.
2. If `current->left` evaluates strictly to null, the local left subspace is empty. Evaluate `current->val` and structurally transition right: `current = current->right`.
3. If a left subspace mathematically exists, geometrically discover the **Inorder Predecessor** (the structurally absolute rightmost node bounded within the left subtree).
4. If the Predecessor's right bound is null:
   - Temporally thread it to `current` (`pre->right = current`).
   - Penetrate deeper left: `current = current->left`.
5. If the Predecessor's right bound already explicitly points to `current`, the threading is fully evaluated.
   - Sever the thread to strictly restore original graph topology (`pre->right = nullptr`).
   - Evaluate `current->val`.
   - Transition right: `current = current->right`.

---

## Code Implementation

```cpp
#include <vector>

using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

vector<int> morrisInorderTraversal(TreeNode* root) {
    vector<int> res;
    TreeNode* current = root;
    
    while (current != nullptr) {
        if (current->left == nullptr) {
            // No structural predecessor subspace; evaluate directly
            res.push_back(current->val);
            current = current->right;
        } else {
            // Geometrically locate absolute Inorder Predecessor
            TreeNode* pre = current->left;
            while (pre->right != nullptr && pre->right != current) {
                pre = pre->right;
            }
            
            // Thread creation: Establish temporal back-link
            if (pre->right == nullptr) {
                pre->right = current;
                current = current->left;
            } 
            // Thread evaluation: Re-link destruction and spatial continuation
            else {
                pre->right = nullptr; // Mathematically restore invariant
                res.push_back(current->val);
                current = current->right;
            }
        }
    }
    
    return res;
}
```

> [!warning]
> Morris Traversal actively mutates graph topology during execution. If multiple concurrent threads execute this algorithm on a shared tree instance, massive synchronization collisions and cycle loops are structurally guaranteed.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. Although locating the theoretical predecessor mandates a nested sequence, each edge in the entire geometry is traversed structurally exactly twice (once to thread, once to evaluate). Thus, performance is strictly $2N \implies \mathcal{O}(N)$.
- **Space Complexity:** $\mathcal{O}(1)$ supplementary structure. Bypasses the call stack completely utilizing explicit pointer mutation.
