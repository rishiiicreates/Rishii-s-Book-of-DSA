---
type: concept
tags: [heap, dsa, cpp]
date: 2026-06-30
---
# Merge k sorted

## Problem Statement
Merge `k` sorted [[Linked List]]s (or arrays) and return them as one sorted list.

## Approach / Intuition
To efficiently merge `k` lists, use a min-[[Heap]]. Push the first element (or head node) of all `k` lists into the heap. Then, iteratively extract the minimum element from the heap, add it to the merged result, and push the next element from the same list that the extracted element belonged to. Repeat this until the heap is empty.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log k) where N is the total number of elements across all lists.
- **[[Space Complexity]]:** O(k) for the heap.

## Sample Code
```cpp
#include <vector>
#include <queue>

using namespace std;

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(NULL) {}
};

struct CompareNode {
    bool operator()(ListNode* const& p1, ListNode* const& p2) {
        return p1->val > p2->val;
    }
};

ListNode* mergeKLists(vector<ListNode*>& lists) {
    priority_queue<ListNode*, vector<ListNode*>, CompareNode> minHeap;
    
    for (auto list : lists) {
        if (list) minHeap.push(list);
    }
    
    ListNode dummy(0);
    ListNode* tail = &dummy;
    
    while (!minHeap.empty()) {
        ListNode* smallest = minHeap.top();
        minHeap.pop();
        
        tail->next = smallest;
        tail = tail->next;
        
        if (smallest->next) {
            minHeap.push(smallest->next);
        }
    }
    
    return dummy.next;
}
```

## New Keywords / STL Used
`std::priority_queue`, custom comparator `operator()`

## Edge Cases
- `k` is 0 (empty input vector).
- Some or all of the input lists are empty.
