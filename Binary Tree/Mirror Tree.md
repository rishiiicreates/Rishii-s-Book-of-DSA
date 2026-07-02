---
type: concept
tags: [binary-tree, recursion, cpp]
date: 2026-06-30
---
# Mirror Tree (Invert Binary Tree)

## Problem Statement
Given the root of a binary tree, mathematically invert the tree such that for every node $N_i$ in the tree, its left child and right child are swapped. The resulting tree is the topological mirror image of the original tree.

---

## Approach: Post-Order Traversal (Recursion)

The operation of mirroring a tree exhibits strictly optimal substructure. A tree rooted at node $U$ is perfectly mirrored if and only if:
1. The left subtree of $U$ is perfectly mirrored.
2. The right subtree of $U$ is perfectly mirrored.
3. The spatial pointers `U->left` and `U->right` are subsequently swapped.

This bottom-up resolution maps elegantly to a [[Post-Order Traversal]].

1. **Base Case:** If the current node is `nullptr`, the mirror of an empty tree is an empty tree. Return `nullptr`.
2. **Recursive Step:** Recursively mirror the left subtree and the right subtree.
3. **Transformation:** Swap the pointers to the left and right children of the current node using `std::swap`.
4. **Return State:** Return the current node, whose subtrees are now fully inverted.

> [!tip]
> While a Pre-Order traversal (swap first, then recurse) is also mathematically valid, the Post-Order traversal is often preferred as it guarantees that child topologies are fully resolved before altering the parent's structure.

---

## Code Implementation

```cpp
#include <algorithm>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

void mirrorTree(Node* node) {
    // Base Case: Empty subtree
    if (node == nullptr) {
        return;
    }
    
    // Post-Order Step 1: Recursively mirror the left subtree
    mirrorTree(node->left);
    
    // Post-Order Step 2: Recursively mirror the right subtree
    mirrorTree(node->right);
    
    // Post-Order Step 3: Swap the topological pointers
    swap(node->left, node->right);
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. The algorithm visits every node exactly once to perform a constant-time pointer swap operation. Therefore, the total processing time is strictly linear.
- **Space Complexity:** $O(H)$, where $H$ is the height of the tree, stemming from the implicit recursion call stack. For a perfectly balanced tree, this is $O(\log N)$. For a skewed pathological tree, this degrades to $O(N)$.
