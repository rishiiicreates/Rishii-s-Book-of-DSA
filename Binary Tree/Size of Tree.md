---
type: concept
tags: [binary-tree, cpp, math, recursion]
date: 2026-07-01
---
# Size of a Binary Tree

## Problem Statement
Given the geometric root node of a Binary Tree $T$, mathematically compute the absolute scalar cardinality of its topological vertices, defined as the `Size` of the tree $|T|$.

---

## Approach: Recursive Isomorphism (DFS)

A Binary Tree $T$ is mathematically axiomatized as either an empty set $\emptyset$ or a root vertex $v$ connecting to two disjoint subtrees: a left subtree $T_L$ and a right subtree $T_R$.
By topological induction, the scalar cardinality of the entire tree strictly conforms to the geometric recurrence relation:
$$ |T| = \begin{cases} 0 & \text{if } T = \emptyset \\ 1 + |T_L| + |T_R| & \text{otherwise} \end{cases} $$

This maps perfectly to a Depth-First Search (DFS) recursive traversal, aggregating the topological count of all descendent vertices iteratively bounded by the geometric leaf condition.

---

## Code Implementation

```cpp
#include <iostream>

using namespace std;

// Topological Node Structure
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

int getSize(TreeNode* root) {
    // Base Case Topology (Empty Set)
    if (root == nullptr) {
        return 0;
    }
    
    // Recursive Geometric Aggregation
    int left_cardinality = getSize(root->left);
    int right_cardinality = getSize(root->right);
    
    return 1 + left_cardinality + right_cardinality;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ strict linear constraint. The algorithm executes a mathematically exhaustive traversal, visiting every topological vertex exactly once.
- **Space Complexity:** $O(H)$ where $H$ is the absolute geometric height of the tree. This bounds the maximum depth of the implicit OS call-stack. In the worst case (skewed degenerate topology), $H = N$. In an optimally balanced topology, $H = \log_2 N$.

> [!tip]
> **Iterative BFS Alternative:** For architectures with extremely constrained bounds on call-stack depth (such as embedded kernels), recursive DFS mapping may trigger a catastrophic Stack Overflow on highly skewed inputs. Inverting this to a Breadth-First Search (BFS) topology utilizing a heap-allocated `std::queue` resolves this limit, achieving identical $O(N)$ execution mapped purely to the generic Heap.
