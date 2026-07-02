---
type: concept
tags: [heap, cpp, kth-element]
date: 2026-06-30
---
# Kth Largest or Smallest Element

## Problem Statement
Given an unsorted array of integers, efficiently find the exact K-th largest (or smallest) element directly.

## Approach / Intuition
To find the K-th largest explicitly, heavily maintain a Min Heap strictly of size K. When aggressively processing the array, push elements in and actively pop when the size mathematically exceeds K. By discarding smaller elements actively, the top of the Min Heap inevitably holds the exact required target via [[Bounded Capacity]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log K)
- **[[Space Complexity]]:** O(K)

## Sample Code
```cpp
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
None

## Edge Cases
K equals exactly the array size length, K fundamentally equals 1, deeply negative value arrays.
