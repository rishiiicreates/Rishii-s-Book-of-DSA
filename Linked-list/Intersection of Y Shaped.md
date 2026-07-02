---
type: concept
tags: [linked-list, cpp, two-pointers]
date: 2026-06-30
---
# Intersection of Y Shaped

## Problem Statement
Given the heads of two singly linked-lists `headA` and `headB`, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return `null`.

## Approach / Intuition
We can solve this efficiently using [[Two Pointers]]. We initialize two pointers, `ptr1` at `headA` and `ptr2` at `headB`. We traverse the lists. When a pointer reaches the end of its list, we redirect it to the head of the other list. This way, both pointers will traverse the exact same total distance ($L_A + L_B$) and will meet at the intersection node on the second traversal, or they will both become `nullptr` simultaneously if there is no intersection.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N + M)$
- **[[Space Complexity]]:** $O(1)$

## Sample Code
```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
    if (!headA || !headB) return nullptr;
    
    ListNode *ptr1 = headA;
    ListNode *ptr2 = headB;
    
    while (ptr1 != ptr2) {
        ptr1 = ptr1 ? ptr1->next : headB;
        ptr2 = ptr2 ? ptr2->next : headA;
    }
    
    return ptr1;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
- No intersection between the two lists
- Intersection at the very first node
- Lists of vastly different lengths
