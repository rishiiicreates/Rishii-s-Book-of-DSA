---
type: concept
tags: [linked-list, cpp, pointer-manipulation]
date: 2026-06-30
---
# Reverse a Singly Linked List

## Problem Statement
Given the head of a singly linked list, reverse the list and return the new head.

## Approach / Intuition
Iterate through the list while maintaining three pointers: `prev`, `curr`, and `next`. At each step, save the `next` node, reverse the `curr->next` pointer to point to `prev`, and then shift `prev` and `curr` one step forward. This is a fundamental [[Pointer Manipulation]] exercise.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n)
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    while (curr != nullptr) {
        ListNode* nextTemp = curr->next;
        curr->next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
Empty list, list with a single node.
