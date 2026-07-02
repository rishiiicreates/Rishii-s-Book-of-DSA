---
type: concept
tags: [linked-list, cpp, math]
date: 2026-06-30
---
# Add Two Numbers

## Problem Statement
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

## Approach / Intuition
Since the digits are stored in reverse order, the head of the list represents the least significant digit. We can iterate through both lists simultaneously, adding the values of the nodes along with any [[Carry]] from the previous addition. We create new nodes for the sum and append them to a dummy head list. We continue this process until both lists are exhausted and the carry is zero.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(\max(N, M))$
- **[[Space Complexity]]:** $O(\max(N, M))$ for the resulting list

## Sample Code
```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    ListNode* dummy = new ListNode(0);
    ListNode* temp = dummy;
    int carry = 0;
    
    while (l1 != nullptr || l2 != nullptr || carry) {
        int sum = 0;
        if (l1 != nullptr) {
            sum += l1->val;
            l1 = l1->next;
        }
        if (l2 != nullptr) {
            sum += l2->val;
            l2 = l2->next;
        }
        
        sum += carry;
        carry = sum / 10;
        ListNode* node = new ListNode(sum % 10);
        temp->next = node;
        temp = temp->next;
    }
    
    ListNode* res = dummy->next;
    delete dummy;
    return res;
}
```

## New Keywords / STL Used
`new`, `delete`

## Edge Cases
- One list is much longer than the other
- The sum results in an extra carry digit at the end (e.g., $99 + 1 = 100$)
- Lists representing 0
