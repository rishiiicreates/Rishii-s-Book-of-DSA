# Linked List

## Problem Statement
- a foundational data structure consisting of a sequence of nodes, where each node contains data and a reference (link) to the next node in the sequence.

## Approach / Intuition
- unlike arrays, a [[Linked List]] does not store elements in contiguous memory locations. This allows for efficient insertions and deletions without shifting elements, though it sacrifices random access, requiring O(n) time to traverse to a specific node using [[Pointer Manipulation]].

## Time & Space Complexity
- **[[time Complexity]]:** O(1) for insertion/deletion at known position, O(n) for search/access
- **[[space Complexity]]:** O(n) to store the list

## Sample Code
```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};
```

## New Keywords / STL Used
- `struct`, `nullptr`

## Edge Cases
- empty list (null head), single node list, memory leaks on deletion.

NEXT: [[Index]]
