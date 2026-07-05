# Red-Black Tree

## Problem Statement
- create a self-balancing binary search tree structure that offers faster insertions and deletions than AVL trees by relaxing the strict height balancing.

## Approach / Intuition
- a [[Red-Black Tree]] colors each node red or black and enforces rules (e.g., no two adjacent red nodes, same number of black nodes on all paths to leaves). This guarantees O(log N) height while requiring fewer [[Tree Rotations]] than AVL.

## Time & Space Complexity
- **[[time Complexity]]:** O(log N) for search, insert, and delete.
- **[[space Complexity]]:** O(N)

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
- root must always be black
- handling newly inserted nodes (always RED initially)

NEXT: [[Index]]
