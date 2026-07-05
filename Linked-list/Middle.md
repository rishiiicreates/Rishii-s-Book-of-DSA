# Middle of the Linked List

## Problem Statement
- find the middle node of a singly linked list. If there are two middle nodes (even length list), return the second middle node.

## Approach / Intuition
- use the Tortoise and Hare approach (fast and slow [[Two Pointers]]). Move the slow pointer by one step and the fast pointer by two steps. When the fast pointer reaches the end of the list, the slow pointer will be exactly at the middle node.

## Time & Space Complexity
- **[[time Complexity]]:** O(n)
- **[[space Complexity]]:** O(1)

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
- none specific

## Edge Cases
- empty list, single node list, even number of nodes, odd number of nodes.

NEXT: [[Index]]
