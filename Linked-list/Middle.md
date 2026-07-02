---
type: concept
tags: [linked-list, cpp, two-pointers]
date: 2026-06-30
---
# Middle of the Linked List

## Problem Statement
Find the middle node of a singly linked list. If there are two middle nodes (even length list), return the second middle node.

## Approach / Intuition
Use the Tortoise and Hare approach (fast and slow [[Two Pointers]]). Move the slow pointer by one step and the fast pointer by two steps. When the fast pointer reaches the end of the list, the slow pointer will be exactly at the middle node.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n)
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
ListNode* middleNode(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast != nullptr && fast->next != nullptr) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
Empty list, single node list, even number of nodes, odd number of nodes.
