---
type: concept
tags: [linked-list, cpp, cycle-detection, floyd]
date: 2026-06-30
---
# Detect Cycle

## Problem Statement
Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

## Approach / Intuition
The most efficient way to detect a cycle is using [[Floyd's Cycle Detection]] algorithm, also known as the tortoise and hare algorithm. We maintain two pointers, a `slow` pointer that moves one step at a time, and a `fast` pointer that moves two steps. If there is a cycle, the `fast` pointer will eventually meet the `slow` pointer. If the `fast` pointer reaches the end of the list (`nullptr`), there is no cycle.

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

bool hasCycle(ListNode *head) {
    if (!head || !head->next) return false;
    
    ListNode* slow = head;
    ListNode* fast = head;
    
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        
        if (slow == fast) {
            return true;
        }
    }
    
    return false;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
- Empty list or single node without a cycle
- Single node pointing to itself
- Cycle starting at the head
