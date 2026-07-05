# Reverse a Doubly Linked List

## Problem Statement
- given the head of a doubly linked list, reverse the list by updating the `prev` and `next` pointers.

## Approach / Intuition
- iterate through the [[Doubly Linked List]]. For each node, swap its `prev` and `next` pointers. Continue traversing using what is now the `prev` pointer (since it holds the original next node). Return the last non-null node encountered as the new head.

## Time & Space Complexity
- **[[time Complexity]]:** O(n)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
DoublyNode* reverseDoublyList(DoublyNode* head) {
    if (!head || !head->next) return head;
    DoublyNode* curr = head;
    DoublyNode* newHead = nullptr;
    while (curr != nullptr) {
        DoublyNode* temp = curr->prev;
        curr->prev = curr->next;
        curr->next = temp;
        newHead = curr;
        curr = curr->prev;
    }
    return newHead;
}
```

## New Keywords / STL Used
- none specific

## Edge Cases
- empty list, single node list.

NEXT: [[Index]]
