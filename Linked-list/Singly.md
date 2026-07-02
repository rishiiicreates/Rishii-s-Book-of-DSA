---
type: concept
tags: [linked-list, cpp, traversal]
date: 2026-06-30
---
# Singly Linked List

## Problem Statement
Implement basic operations (traversal, insertion, deletion) on a singly linked list where nodes only point forward to the next node.

## Approach / Intuition
Perform a standard [[Linked List]] traversal using a `while` loop, checking for `node != nullptr`. To insert or delete, you typically need a reference to the previous node to adjust the `next` pointers properly, making careful [[Pointer Manipulation]] essential.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n) for traversal, O(1) for insertion/deletion if pointer to previous node is known
- **[[Space Complexity]]:** O(1) auxiliary space

## Sample Code
```cpp
void traverse(ListNode* head) {
    ListNode* curr = head;
    while (curr != nullptr) {
        curr = curr->next;
    }
}

ListNode* insertAtHead(ListNode* head, int val) {
    ListNode* newNode = new ListNode(val);
    newNode->next = head;
    return newNode;
}
```

## New Keywords / STL Used
`new`

## Edge Cases
Inserting into an empty list, deleting the head node, deleting the tail node.
