---
type: concept
tags: [heap, dsa, cpp]
date: 2026-06-30
---
# Check for Heap

## Problem Statement
Given a [[Binary Tree]] or an [[Array]], check if it satisfies the properties of a max-heap or min-heap.

## Approach / Intuition
For an array representation of a tree, for every node at index `i`, its children are at `2*i + 1` and `2*i + 2`. To verify it is a valid max-heap, we just need to iterate through the array and check if the value at node `i` is greater than or equal to the values at its child indices. If any child is greater, it violates the [[Heap]] property.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) where N is the number of nodes.
- **[[Space Complexity]]:** O(1) iteratively, or O(H) recursively where H is tree height.

## Sample Code
```cpp
#include <vector>

using namespace std;

bool isMaxHeap(const vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i <= (n - 2) / 2; i++) {
        if (arr[2 * i + 1] > arr[i]) {
            return false;
        }
        if (2 * i + 2 < n && arr[2 * i + 2] > arr[i]) {
            return false;
        }
    }
    return true;
}
```

## New Keywords / STL Used
None

## Edge Cases
- Empty array or array with one element (always a heap).
- Array with negative numbers or duplicates.
