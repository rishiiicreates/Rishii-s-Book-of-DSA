# Rotate List

## Problem Statement
- given the `head` of a linked list, rotate the list to the right by `k` places.

## Approach / Intuition
- first, traverse the list to find its full length and connect the tail to the head, temporarily forming a circular linked list. Calculate the effective rotations using `k % length`. Locate the new tail at position `length - k - 1`, break the loop, and set its adjacent node as the new head. This utilizes basic but robust [[List Traversal]].

## Time & Space Complexity
- **[[time Complexity]]:** O(N)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
ListNode* rotateRight(ListNode* head, int k) {
    if (!head || !head->next || k == 0) return head;
    ListNode* curr = head;
    int len = 1;
    while (curr->next) {
        curr = curr->next;
        len++;
    }
    curr->next = head;
    k = k % len;
    k = len - k;
    while (k--) curr = curr->next;
    head = curr->next;
    curr->next = nullptr;
    return head;
}
```

## New Keywords / STL Used
- none

## Edge Cases
- empty list, single node list, `k` strictly matching a multiple of the list length.

NEXT: [[Index]]
