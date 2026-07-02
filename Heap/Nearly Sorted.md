---
type: concept
tags: [heap, dsa, cpp]
date: 2026-06-30
---
# Nearly Sorted

## Problem Statement
Sort an [[Array]] where each element is at most `k` positions away from its target position in the sorted array.

## Approach / Intuition
Since every element is at most `k` places away, the minimum element must be among the first `k+1` elements. We can maintain a min-[[Heap]] of size `k+1`. We insert the first `k+1` elements, then iteratively pop the minimum to place it in the correct position in the array, and push the next element from the array into the heap.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log k)
- **[[Space Complexity]]:** O(k)

## Sample Code
```cpp
#include <vector>
#include <queue>

using namespace std;

void sortKSortedArray(vector<int>& arr, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap;
    int n = arr.size();
    
    for (int i = 0; i <= min(k, n - 1); i++) {
        minHeap.push(arr[i]);
    }
    
    int index = 0;
    for (int i = k + 1; i < n; i++) {
        arr[index++] = minHeap.top();
        minHeap.pop();
        minHeap.push(arr[i]);
    }
    
    while (!minHeap.empty()) {
        arr[index++] = minHeap.top();
        minHeap.pop();
    }
}
```

## New Keywords / STL Used
`std::priority_queue`, `std::greater`, `std::min`

## Edge Cases
- `k = 0` (array is already sorted).
- `k >= n` (equivalent to standard heap sort).
