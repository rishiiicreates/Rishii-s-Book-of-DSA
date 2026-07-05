# Sort K-Sorted Array

## Problem Statement
- efficiently sort an array where every element is practically at most `K` discrete places strictly away from its finalized target sorted position.

## Approach / Intuition
- maintain a resilient Min Heap spanning mathematically exactly `K + 1` elements tightly. Because an element's correct positional order is heavily guaranteed within this window range, aggressively pushing array elements iteratively and popping the minimum instantly yields the absolute globally correct order, exhibiting highly localized [[Windowed Sorting]].

## Time & Space Complexity
- **[[time Complexity]]:** O(N log K)
- **[[space Complexity]]:** O(K)

## Sample Code
```cpp
vector<int> sortKSortedArray(vector<int>& arr, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap;
    vector<int> res;
    for (int i = 0; i < arr.size(); i++) {
        minHeap.push(arr[i]);
        if (minHeap.size() > k) {
            res.push_back(minHeap.top());
            minHeap.pop();
        }
    }
    while (!minHeap.empty()) {
        res.push_back(minHeap.top());
        minHeap.pop();
    }
    return res;
}
```

## New Keywords / STL Used
- none

## Edge Cases
- completely native sorted arrays running baseline, `K = 0` (requiring absolutely zero shifts), `K = N` (mathematically degrading to standard heap sort).

NEXT: [[Index]]
