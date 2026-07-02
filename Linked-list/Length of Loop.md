---
type: concept
tags: [linked-list, cpp, cycle-detection, math]
date: 2026-06-30
---
# Length of Loop

## Problem Statement
Given the head of a linked list, determine whether the list contains a loop. If a loop is present, return the number of nodes in the loop. Otherwise, return 0.

## Approach / Intuition
First, we use [[Floyd's Cycle Detection]] with a `slow` and `fast` pointer to find if a cycle exists. Once they meet, they are guaranteed to be inside the loop. To find the length of the loop, we keep one pointer fixed at the meeting point and move another pointer step by step, counting the number of nodes until it reaches the fixed pointer again.

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

int countNodesInLoop(ListNode *head) {
    ListNode *slow = head;
    ListNode *fast = head;
    
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        
        if (slow == fast) {
            int count = 1;
            ListNode *temp = slow;
            while (temp->next != slow) {
                count++;
                temp = temp->next;
            }
            return count;
        }
    }
    
    return 0;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
- No loop in the list
- The whole list forms a loop (circular linked list)
- The loop consists of only one node
