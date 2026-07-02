---
type: concept
tags: [binary_tree, bst, cpp, red-black, balanced-tree]
date: 2026-06-30
---
# Red-Black Insert or Delete

## Problem Statement
Implement the insertion and deletion operations in a Red-Black Tree, maintaining its color properties and self-balancing characteristics.

## Approach / Intuition
A [[Red-Black Tree]] ensures the path from the root to the farthest leaf is no more than twice as long as the path to the nearest leaf. It uses nodes colored Red or Black. The standard BST insert adds a node colored Red. We then fix violations of Red-Black properties (e.g., two consecutive red nodes) by performing color flips and [[Tree Rotations]]. Similar complex balancing occurs during deletion to maintain the same "black-height" on all paths.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(\log N)$ for both insert and delete.
- **[[Space Complexity]]:** $O(\log N)$ for the recursion stack or $O(1)$ iteratively with parent pointers.

## Sample Code
```cpp
enum Color { RED, BLACK };

struct Node {
    int data;
    bool color;
    Node *left, *right, *parent;
    Node(int data) : data(data), color(RED), left(nullptr), right(nullptr), parent(nullptr) {}
};

void rotateLeft(Node *&root, Node *&pt) {
    Node *pt_right = pt->right;
    pt->right = pt_right->left;
    
    if (pt->right != nullptr)
        pt->right->parent = pt;
        
    pt_right->parent = pt->parent;
    
    if (pt->parent == nullptr)
        root = pt_right;
    else if (pt == pt->parent->left)
        pt->parent->left = pt_right;
    else
        pt->parent->right = pt_right;
        
    pt_right->left = pt;
    pt->parent = pt_right;
}

// Minimal placeholder for complex RB-Tree Fixup logic
void fixViolation(Node *&root, Node *&pt) {
    Node *parent_pt = nullptr;
    Node *grand_parent_pt = nullptr;
    
    while ((pt != root) && (pt->color != BLACK) && (pt->parent->color == RED)) {
        parent_pt = pt->parent;
        grand_parent_pt = pt->parent->parent;
        // Fixup cases involving uncles, recoloring, and rotations go here.
        break; // Shortened for brevity
    }
    root->color = BLACK;
}
```

## New Keywords / STL Used
`enum`

## Edge Cases
- Inserting into an empty tree (root must be colored black)
- Consecutive red nodes (violation)
- Deleting a black node (disrupts black-height)
