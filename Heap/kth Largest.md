---
type: concept
tags: [heap, dsa, cpp]
date: 2026-06-30
---
# kth Largest

## Problem Statement
Find the kth largest element in an unsorted [[Array]].

## Approach / Intuition
We can maintain a min-[[Heap]] of size `k`. Iterate through the array and push elements into the heap. If the heap size exceeds `k`, pop the minimum element. After processing all elements, the top of the min-heap will be the kth largest element, because the heap stores the `k` largest elements seen so far, and its root is the smallest among them.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log k)
- **[[Space Complexity]]:** O(k)

## Sample Code
```cpp
#include <vector>
#include <queue>

using namespace std;

int findKthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap;
    for (int num : nums) {
        minHeap.push(num);
        if (minHeap.size() > k) {
            minHeap.pop();
        }
    }
    return minHeap.top();
}
```

## New Keywords / STL Used
`std::priority_queue`, `std::greater`

## Edge Cases
- `k` is equal to the size of the array (find minimum).
- `k` is 1 (find maximum).
- Duplicates in the array.
