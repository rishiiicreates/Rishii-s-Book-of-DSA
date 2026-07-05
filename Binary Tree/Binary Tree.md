# Binary Tree

## Mathematical Definition
- a **Binary Tree** is a hierarchical, acyclic directed graph structurally defined by a strict out-degree constraint: every node $v \in V$ is connected to at most two distinct child nodes (conventionally ordered as *left* and *right*).

- geometrically, a Binary Tree is recursively defined as either:
- the null set (an empty tree $\emptyset$).
- a discrete root node $r$, structurally bound to two disjoint binary trees (the left subtree $T_L$ and the right subtree $T_R$).


## Structural Properties & Invariants

- let $N$ denote the absolute number of nodes, $H$ denote the strict structural height (where a single root node bears height $1$), and $L$ denote the total number of leaf nodes (nodes with out-degree $0$).

- **maximum Node Capacity:** The theoretical maximum number of nodes at geometric depth $d$ (where root is $d=1$) evaluates strictly to $2^{d-1}$.
- **absolute Size Bounds:** A binary tree of height $H$ can contain mathematically between $H$ (a degenerate, skewed tree) and $2^H - 1$ (a strictly perfect binary tree) nodes.
- **height vs Nodes:**
   - minimum theoretical height given $N$ nodes: $H_{min} = \lceil \log_2(N + 1) \rceil$.
   - maximum theoretical height given $N$ nodes: $H_{max} = N$.
- **leaf-to-Internal Relation:** In any strictly non-empty binary tree where every node possesses either exactly $0$ or $2$ children (a strict/full binary tree), the number of leaves $L$ strictly bounds the number of internal nodes $I$ by the invariant: $L = I + 1$.


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


## Common Traversal Taxonomies
- evaluating the sequence of nodes requires algorithmic traversal bounded by the hierarchical geometry.
- **depth-First Search (DFS):** Penetrates structurally downward to absolute leaf nodes before resolving lateral boundaries. Driven theoretically by the Call Stack (LIFO). Taxonomies depend on the exact temporal evaluation of the root $r$ relative to subtrees $T_L, T_R$:
  - **[[preorder]]** (Root, $T_L$, $T_R$)
  - **[[inorder]]** ($T_L$, Root, $T_R$)
  - **[[postorder]]** ($T_L$, $T_R$, Root)
- **breadth-First Search (BFS):** Evaluates strictly via spatial depth layers. Driven theoretically by a Queue (FIFO).
  - **[[level Order]]**

NEXT: [[Index]]
