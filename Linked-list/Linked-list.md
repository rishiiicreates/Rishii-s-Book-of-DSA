---
type: concept
tags: [linked-list, cpp, data-structure]
date: 2026-06-30
---
# Linked List

## Problem Statement
A foundational data structure consisting of a sequence of nodes, where each node contains data and a reference (link) to the next node in the sequence.

## Approach / Intuition
Unlike arrays, a [[Linked List]] does not store elements in contiguous memory locations. This allows for efficient insertions and deletions without shifting elements, though it sacrifices random access, requiring O(n) time to traverse to a specific node using [[Pointer Manipulation]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(1) for insertion/deletion at known position, O(n) for search/access
- **[[Space Complexity]]:** O(n) to store the list

## Sample Code
```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};
```

## New Keywords / STL Used
`struct`, `nullptr`

## Edge Cases
Empty list (null head), single node list, memory leaks on deletion.
