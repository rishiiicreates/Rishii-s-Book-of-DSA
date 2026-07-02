---
type: concept
tags: [linked-list, cpp, merge, priority-queue]
date: 2026-06-30
---
# Merge K Sorted Lists

## Problem Statement
You are given an array of `k` linked-lists, where each linked-list is inherently sorted in ascending order. Merge all the linked-lists into one optimally sorted linked-list and return it.

## Approach / Intuition
Leverage a min-heap (priority queue) to maintain the smallest available nodes across all `k` lists simultaneously. Pop the absolute minimum, append it to the aggregated result list, and push its next consecutive node back into the heap. This showcases a perfect real-world application of a [[Priority Queue]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log k)
- **[[Space Complexity]]:** O(k)

## Sample Code
```cpp
struct compare {
    bool operator()(const ListNode* l, const ListNode* r) {
        return l->val > r->val;
    }
};

ListNode* mergeKLists(vector<ListNode*>& lists) {
    priority_queue<ListNode*, vector<ListNode*>, compare> pq;
    for (ListNode* list : lists) {
        if (list) pq.push(list);
    }
    ListNode dummy(0);
    ListNode* tail = &dummy;
    while (!pq.empty()) {
        ListNode* minNode = pq.top();
        pq.pop();
        tail->next = minNode;
        tail = tail->next;
        if (minNode->next) {
            pq.push(minNode->next);
        }
    }
    return dummy.next;
}
```

## New Keywords / STL Used
`std::priority_queue`

## Edge Cases
Empty array of lists, arrays sporadically containing null lists.
