# Reverse Nodes in k-Group

## Problem Statement
- given the head of a linked list, reverse the nodes of the list `k` at a time, and return the modified list. If the number of nodes is not a multiple of `k` then left-out nodes in the end should remain as it is.

## Approach / Intuition
- first, count the total nodes to know how many full groups of `k` exist. Use a dummy node to ease head manipulation. For each group, use standard [[Pointer Manipulation]] to reverse `k` nodes, carefully re-linking the ends of the reversed segment to the previous and next parts of the list.

## Time & Space Complexity
- **[[time Complexity]]:** O(n)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
ListNode* reverseKGroup(ListNode* head, int k) {
    if (!head || k == 1) return head;
    int count = 0;
    ListNode* curr = head;
    while (curr) { count++; curr = curr->next; }
    
    ListNode dummy(0);
    dummy.next = head;
    ListNode* prevGroupTail = &dummy;
    curr = head;
    
    while (count >= k) {
        ListNode* prev = nullptr;
        ListNode* groupHead = curr;
        for (int i = 0; i < k; ++i) {
            ListNode* nextTemp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = nextTemp;
        }
        prevGroupTail->next = prev;
        groupHead->next = curr;
        prevGroupTail = groupHead;
        count -= k;
    }
    return dummy.next;
}
```

## New Keywords / STL Used
- none specific

## Edge Cases
- `k=1`, `k` greater than list length, list length is a multiple of `k`, empty list.

NEXT: [[Index]]
