# K’th largest in a stream

## Problem Statement
- design a class to find the `k`th largest element in a stream. Note that it is the `k`th largest element in the sorted order, not the `k`th distinct element.

## Approach / Intuition
- use a min-[[Heap]] of size `k` to continuously track the `k` largest elements seen so far. When a new element comes in, we add it to the min-heap. If the heap size exceeds `k`, we pop the minimum element. The top of the min-heap will always represent the `k`th largest element of the stream at any given time.

## Time & Space Complexity
- **[[time Complexity]]:** O(log k) for each `add` operation. O(N log k) for initial construction.
- **[[space Complexity]]:** O(k)

## Sample Code
```cpp
#include <vector>
#include <queue>

using namespace std;

class KthLargest {
    priority_queue<int, vector<int>, greater<int>> minHeap;
    int k;
public:
    KthLargest(int k, vector<int>& nums) {
        this->k = k;
        for (int num : nums) {
            add(num);
        }
    }
    
    int add(int val) {
        minHeap.push(val);
        if (minHeap.size() > k) {
            minHeap.pop();
        }
        return minHeap.top();
    }
};
```

## New Keywords / STL Used
- `std::priority_queue`, `std::greater`

## Edge Cases
- adding elements when the heap has fewer than `k` elements initially.
- stream contains negative numbers or duplicates.

NEXT: [[Index]]
