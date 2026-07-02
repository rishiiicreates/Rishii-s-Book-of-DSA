---
type: concept
tags: [linked-list, cpp, two-pointers]
date: 2026-06-30
---
# Remove Nth from End

## Problem Statement
Given the `head` of a linked list, remove the $n^{th}$ node from the end of the list and return its head.

## Approach / Intuition
We use a [[Two Pointers]] approach with a `fast` and a `slow` pointer. First, we move the `fast` pointer $n$ steps ahead. If the `fast` pointer becomes null, it means the head itself needs to be removed. Otherwise, we move both `slow` and `fast` pointers one step at a time until `fast->next` is null. At this point, the `slow` pointer is right before the node to be removed. We then adjust the next pointer of `slow` to skip the $n^{th}$ node.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(1)$

## Sample Code
```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode* fast = head;
    ListNode* slow = head;
    
    for (int i = 0; i < n; i++) {
        fast = fast->next;
    }
    
    if (!fast) {
        ListNode* temp = head->next;
        delete head;
        return temp;
    }
    
    while (fast->next) {
        fast = fast->next;
        slow = slow->next;
    }
    
    ListNode* nodeToDelete = slow->next;
    slow->next = slow->next->next;
    delete nodeToDelete;
    
    return head;
}
```

## New Keywords / STL Used
`delete`

## Edge Cases
- Linked list with only 1 element
- Removing the head of the list
- Removing the last node
