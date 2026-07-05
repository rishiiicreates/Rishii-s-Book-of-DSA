# Reverse a Singly Linked List

## Problem Statement
- given the head of a singly linked list, reverse the list and return the new head.

## Approach / Intuition
- iterate through the list while maintaining three pointers: `prev`, `curr`, and `next`. At each step, save the `next` node, reverse the `curr->next` pointer to point to `prev`, and then shift `prev` and `curr` one step forward. This is a fundamental [[Pointer Manipulation]] exercise.

## Time & Space Complexity
- **[[time Complexity]]:** O(n)
- **[[space Complexity]]:** O(1)

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
- none specific

## Edge Cases
- empty list, list with a single node.

NEXT: [[Index]]
