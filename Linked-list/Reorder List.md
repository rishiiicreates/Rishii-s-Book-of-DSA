---
type: concept
tags: [linked-list, cpp, two-pointers]
date: 2026-06-30
---
# Reorder List

## Problem Statement
Given a singly linked list `L0 -> L1 -> ... -> Ln-1 -> Ln`, reorder it to `L0 -> Ln -> L1 -> Ln-1 -> L2 -> Ln-2 -> ...`

## Approach / Intuition
This problem combines three techniques. First, use a fast and slow pointer to find the [[Middle]] of the list. Second, [[Reverse]] the second half of the list. Finally, merge the two halves by alternating nodes from each half using a [[Two Pointers]] approach.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n)
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
void reorderList(ListNode* head) {
    if (!head || !head->next) return;
    ListNode *slow = head, *fast = head->next;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    
    ListNode* second = slow->next;
    slow->next = nullptr;
    ListNode* prev = nullptr;
    while (second) {
        ListNode* nextTemp = second->next;
        second->next = prev;
        prev = second;
        second = nextTemp;
    }
    second = prev;
    ListNode* first = head;
    
    while (second) {
        ListNode* temp1 = first->next;
        ListNode* temp2 = second->next;
        first->next = second;
        second->next = temp1;
        first = temp1;
        second = temp2;
    }
}
```

## New Keywords / STL Used
None specific

## Edge Cases
Empty list, list with 1 or 2 nodes, list with odd/even number of nodes.
