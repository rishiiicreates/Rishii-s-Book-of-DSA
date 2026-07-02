---
type: concept
tags: [tree, binary-tree, cpp, math, graph-theory]
date: 2026-06-30
---
# Binary Tree

## Mathematical Definition
A **Binary Tree** is a hierarchical, acyclic directed graph structurally defined by a strict out-degree constraint: every node $v \in V$ is connected to at most two distinct child nodes (conventionally ordered as *left* and *right*). 

Geometrically, a Binary Tree is recursively defined as either:
1. The null set (an empty tree $\emptyset$).
2. A discrete root node $r$, structurally bound to two disjoint binary trees (the left subtree $T_L$ and the right subtree $T_R$).

---

## Structural Properties & Invariants

Let $N$ denote the absolute number of nodes, $H$ denote the strict structural height (where a single root node bears height $1$), and $L$ denote the total number of leaf nodes (nodes with out-degree $0$).

1. **Maximum Node Capacity:** The theoretical maximum number of nodes at geometric depth $d$ (where root is $d=1$) evaluates strictly to $2^{d-1}$.
2. **Absolute Size Bounds:** A binary tree of height $H$ can contain mathematically between $H$ (a degenerate, skewed tree) and $2^H - 1$ (a strictly perfect binary tree) nodes.
3. **Height vs Nodes:**
   - Minimum theoretical height given $N$ nodes: $H_{min} = \lceil \log_2(N + 1) \rceil$.
   - Maximum theoretical height given $N$ nodes: $H_{max} = N$.
4. **Leaf-to-Internal Relation:** In any strictly non-empty binary tree where every node possesses either exactly $0$ or $2$ children (a strict/full binary tree), the number of leaves $L$ strictly bounds the number of internal nodes $I$ by the invariant: $L = I + 1$.

---

## Code Implementation (Structural Node Definition)

```cpp
#include <cstddef>

// Mathematical abstraction of a Binary Tree structural unit
struct TreeNode {
    int val;                 // Theoretical scalar value
    TreeNode* left;          // Spatial pointer to left geometric subspace
    TreeNode* right;         // Spatial pointer to right geometric subspace
    
    // Explicit structural initialization
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

> [!important]
> For rigorous C++ implementations, it is mathematically critical to default unassigned spatial bounds to `nullptr`. Leaving geometric bounds dangling fundamentally breaks traversal invariants and results in undefined structural behavior (segfaults) during memory evaluation.

---

## Common Traversal Taxonomies
Evaluating the sequence of nodes requires algorithmic traversal bounded by the hierarchical geometry.
- **Depth-First Search (DFS):** Penetrates structurally downward to absolute leaf nodes before resolving lateral boundaries. Driven theoretically by the Call Stack (LIFO). Taxonomies depend on the exact temporal evaluation of the root $r$ relative to subtrees $T_L, T_R$:
  - **[[Preorder]]** (Root, $T_L$, $T_R$)
  - **[[Inorder]]** ($T_L$, Root, $T_R$)
  - **[[Postorder]]** ($T_L$, $T_R$, Root)
- **Breadth-First Search (BFS):** Evaluates strictly via spatial depth layers. Driven theoretically by a Queue (FIFO).
  - **[[Level Order]]**
