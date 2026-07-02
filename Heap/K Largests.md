---
type: concept
tags: [heap, dsa, cpp]
date: 2026-06-30
---
# K Largests

## Problem Statement
Find the `k` largest elements from an unsorted [[Array]].

## Approach / Intuition
Use a min-[[Heap]] of size `k` to keep track of the largest elements encountered. As we traverse the array, insert the current element into the heap. If the heap size exceeds `k`, remove the top element (which is the smallest in the heap). Finally, the elements remaining in the min-heap are the `k` largest elements.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log k)
- **[[Space Complexity]]:** O(k)

## Sample Code
```cpp
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

vector<int> kLargest(vector<int>& arr, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap;
    for (int num : arr) {
        minHeap.push(num);
        if (minHeap.size() > k) {
            minHeap.pop();
        }
    }
    vector<int> result;
    while (!minHeap.empty()) {
        result.push_back(minHeap.top());
        minHeap.pop();
    }
    reverse(result.begin(), result.end());
    return result;
}
```

## New Keywords / STL Used
`std::priority_queue`, `std::greater`, `std::reverse`

## Edge Cases
- `k` equals the length of the array.
- Array contains duplicates.
