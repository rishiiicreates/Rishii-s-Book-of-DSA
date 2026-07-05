# Range Covering K Sorted Lists

## Problem Statement
- given `k` sorted arrays, find the smallest range `[a, b]` that includes at least one number from each of the `k` lists.

## Approach / Intuition
- use a min-[[Heap]] to keep track of the minimum element across all `k` lists, and a variable to track the maximum element currently in our sliding window. Initialize the heap with the first element of each list. At any point, the current range is `[minHeap.top(), currentMax]`. Try to update the best range if this is smaller. Then pop the minimum element and push the next element from its list. Update `currentMax` if the new element is larger. The process stops when any list is exhausted.

## Time & Space Complexity
- **[[time Complexity]]:** O(N log k) where N is the total number of elements.
- **[[space Complexity]]:** O(k) for the min-heap.

## Sample Code
```cpp
#include <vector>
#include <queue>
#include <climits>

using namespace std;

vector<int> smallestRange(vector<vector<int>>& nums) {
    int k = nums.size();
    using Element = pair<int, pair<int, int>>;
    priority_queue<Element, vector<Element>, greater<Element>> minHeap;
    
    int currentMax = INT_MIN;
    for (int i = 0; i < k; i++) {
        minHeap.push({nums[i][0], {i, 0}});
        currentMax = max(currentMax, nums[i][0]);
    }
    
    int rangeStart = 0, rangeEnd = INT_MAX;
    
    while (true) {
        auto top = minHeap.top();
        minHeap.pop();
        int minVal = top.first;
        int listIdx = top.second.first;
        int eleIdx = top.second.second;
        
        if (currentMax - minVal < rangeEnd - rangeStart) {
            rangeStart = minVal;
            rangeEnd = currentMax;
        }
        
        if (eleIdx + 1 == nums[listIdx].size()) {
            break;
        }
        
        int nextVal = nums[listIdx][eleIdx + 1];
        minHeap.push({nextVal, {listIdx, eleIdx + 1}});
        currentMax = max(currentMax, nextVal);
    }
    
    return {rangeStart, rangeEnd};
}
```

## New Keywords / STL Used
- `std::priority_queue`, `std::pair`, `std::greater`, `INT_MAX`, `INT_MIN`

## Edge Cases
- several lists have identical elements.
- a single element list restricting the advancement of the algorithm early.

NEXT: [[Index]]
