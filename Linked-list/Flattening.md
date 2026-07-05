# Flattening a Linked List

## Problem Statement
- given a linked list where every central node contains standard `next` pointers to adjacent lateral nodes and `bottom` pointers to isolated child linked lists, flatten it heavily into a single-level sorted linked list using only the `bottom` structural pointer.

## Approach / Intuition
- rely on deep recursion to aggressively flatten trailing list nodes first. Iteratively merge the vertical `bottom` linked list actively with the flattened output emerging from the right-side list, cleanly emulating traditional [[Merge Sort]] logic atop raw linked lists.

## Time & Space Complexity
- **[[time Complexity]]:** O(N * M)
- **[[space Complexity]]:** O(N) auxiliary space recursively

## Sample Code
```cpp
Node* mergeTwoLists(Node* a, Node* b) {
    if (!a) return b;
    if (!b) return a;
    Node* res;
    if (a->data < b->data) {
        res = a;
        res->bottom = mergeTwoLists(a->bottom, b);
    } else {
        res = b;
        res->bottom = mergeTwoLists(a, b->bottom);
    }
    res->next = nullptr;
    return res;
}

Node* flatten(Node* root) {
    if (!root || !root->next) return root;
    root->next = flatten(root->next);
    root = mergeTwoLists(root, root->next);
    return root;
}
```

## New Keywords / STL Used
- none

## Edge Cases
- linear lists existing purely down a single `bottom` hierarchy without any `next` elements natively populated.

NEXT: [[Index]]
