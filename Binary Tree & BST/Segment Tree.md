---
type: concept
tags: [binary_tree, bst, cpp, range-queries]
date: 2026-06-30
---
# Segment Tree

## Problem Statement
Efficiently query aggregate functions (like sum, min, max) over array intervals and perform element updates.

## Approach / Intuition
A [[Segment Tree]] is a full binary tree where leaves represent array elements and internal nodes store the merged results of their children. This allows for fast [[Range Queries]] and point updates.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) to build, O(log N) for query and update.
- **[[Space Complexity]]:** O(N) (specifically 4N size for the array representation).

## Sample Code
```cpp
void build(vector<int>& tree, vector<int>& arr, int node, int start, int end) {
    if (start == end) {
        tree[node] = arr[start];
    } else {
        int mid = (start + end) / 2;
        build(tree, arr, 2 * node, start, mid);
        build(tree, arr, 2 * node + 1, mid + 1, end);
        tree[node] = tree[2 * node] + tree[2 * node + 1];
    }
}
```

## New Keywords / STL Used
- `std::vector`

## Edge Cases
- Range query entirely outside the node's interval
- Single element array
