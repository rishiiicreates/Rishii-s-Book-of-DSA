# Doubly and Circular Linked Lists

## Problem Statement
- understand and implement variations of the standard linked list: Doubly Linked Lists (having both `prev` and `next` pointers) and Circular Linked Lists (where the tail points back to the head).

## Approach / Intuition
- a [[Doubly Linked List]] allows bidirectional traversal by maintaining two pointers per node. This makes deletion given only the node pointer O(1). A [[Circular Linked List]] forms a continuous loop, which is useful for applications requiring continuous cycling through elements, like round-robin scheduling.

## Time & Space Complexity
- **[[time Complexity]]:** O(n) for search, O(1) for deletion/insertion at known nodes
- **[[space Complexity]]:** O(n)

## Sample Code
```cpp
struct DoublyNode {
    int val;
    DoublyNode *prev;
    DoublyNode *next;
    DoublyNode(int x) : val(x), prev(nullptr), next(nullptr) {}
};

void insertAfter(DoublyNode* node, int val) {
    if (!node) return;
    DoublyNode* newNode = new DoublyNode(val);
    newNode->next = node->next;
    newNode->prev = node;
    if (node->next) {
        node->next->prev = newNode;
    }
    node->next = newNode;
}
```

## New Keywords / STL Used
- none specific

## Edge Cases
- empty list, list with one node (points to itself in circular), updating head/tail pointers correctly.

NEXT: [[Index]]
