---
type: concept
tags: [binary_tree, bst, cpp, balanced-tree]
date: 2026-06-30
---
# Red-Black Tree

## Problem Statement
Create a self-balancing binary search tree structure that offers faster insertions and deletions than AVL trees by relaxing the strict height balancing.

## Approach / Intuition
A [[Red-Black Tree]] colors each node red or black and enforces rules (e.g., no two adjacent red nodes, same number of black nodes on all paths to leaves). This guarantees O(log N) height while requiring fewer [[Tree Rotations]] than AVL.

## Time & Space Complexity
- **[[Time Complexity]]:** O(log N) for search, insert, and delete.
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
enum Color { RED, BLACK };
struct RBNode {
    int val;
    Color color;
    RBNode *left, *right, *parent;
    RBNode(int x) : val(x), color(RED), left(nullptr), right(nullptr), parent(nullptr) {}
};
```

## New Keywords / STL Used
- `enum`

## Edge Cases
- Root must always be black
- Handling newly inserted nodes (always RED initially)
