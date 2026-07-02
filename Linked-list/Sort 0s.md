---
type: concept
tags: [linked-list, cpp, sorting, dutch-national-flag]
date: 2026-06-30
---
# Sort 0s

## Problem Statement
Given a linked list containing 0s, 1s, and 2s, sort the linked list. (This file represents the 0s part of sorting the linked list values).

## Approach / Intuition
To sort a linked list of 0s, 1s, and 2s, we can use the concept of the [[Dutch National Flag]] algorithm adapted for linked lists. We create three dummy nodes to act as the heads of three separate linked lists (one for 0s, one for 1s, and one for 2s). We traverse the original list and append each node to the corresponding list based on its value. Finally, we concatenate the three lists together.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(1)$ auxiliary space

## Sample Code
```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

ListNode* segregate(ListNode *head) {
    if (!head || !head->next) return head;
    
    ListNode* zeroD = new ListNode(0);
    ListNode* oneD = new ListNode(0);
    ListNode* twoD = new ListNode(0);
    
    ListNode* zero = zeroD, *one = oneD, *two = twoD;
    ListNode* curr = head;
    
    while (curr) {
        if (curr->val == 0) {
            zero->next = curr;
            zero = zero->next;
        } else if (curr->val == 1) {
            one->next = curr;
            one = one->next;
        } else {
            two->next = curr;
            two = two->next;
        }
        curr = curr->next;
    }
    
    zero->next = (oneD->next) ? (oneD->next) : (twoD->next);
    one->next = twoD->next;
    two->next = nullptr;
    
    head = zeroD->next;
    
    delete zeroD;
    delete oneD;
    delete twoD;
    
    return head;
}
```

## New Keywords / STL Used
`new`, `delete`

## Edge Cases
- List consisting entirely of one value
- List missing one of the values (e.g., no 1s)
- Empty list
