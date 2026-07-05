# Median in a stream

## Problem Statement
- design a data structure that supports inserting a stream of integers and retrieving the median of all inserted elements at any time.

## Approach / Intuition
- we can keep the incoming integers sorted by splitting them into a lower half and an upper half using two [[Priority Queue]] instances. The lower half is maintained as a max-heap, while the upper half is a min-heap. We balance them such that their sizes differ by at most 1. The median can then be retrieved in $O(1)$ time by inspecting the top of the heaps: either the average of the two tops if the total count is even, or the top of the larger heap if the total count is odd.

## Time & Space Complexity
- **[[time Complexity]]:** O(log N) for insertion, O(1) for median
- **[[space Complexity]]:** O(N)

## Sample Code
```cpp
#include <queue>
#include <vector>

using namespace std;

class MedianFinder {
    priority_queue<int> max_heap; 
    priority_queue<int, vector<int>, greater<int>> min_heap; 
    
public:
    MedianFinder() {}
    
    void addNum(int num) {
        max_heap.push(num);
        min_heap.push(max_heap.top());
        max_heap.pop();
        
        if (max_heap.size() < min_heap.size()) {
            max_heap.push(min_heap.top());
            min_heap.pop();
        }
    }
    
    double findMedian() {
        if (max_heap.size() > min_heap.size()) {
            return max_heap.top();
        }
        return (max_heap.top() + min_heap.top()) / 2.0;
    }
};
```

## New Keywords / STL Used
- `std::priority_queue`, `std::greater`

## Edge Cases
- extracting median after 1 element
- duplicates in the stream
- large stream of continuously increasing or decreasing numbers

NEXT: [[Index]]
