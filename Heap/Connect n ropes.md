---
type: concept
tags: [heap, dsa, cpp]
date: 2026-06-30
---
# Connect n ropes

## Problem Statement
Given `n` ropes of different lengths, connect them into a single rope with minimum cost. The cost to connect two ropes is equal to the sum of their lengths.

## Approach / Intuition
To minimize the overall cost, we should always connect the two shortest ropes available. We can maintain a min-[[Heap]] containing the lengths of all ropes. Repeatedly extract the two smallest lengths, add them up to find the cost of connecting them, and insert the new combined length back into the heap. Add this cost to the total cost, and repeat until one rope remains.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log N)
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
#include <vector>
#include <queue>

using namespace std;

long long minCost(vector<long long>& arr) {
    priority_queue<long long, vector<long long>, greater<long long>> pq(arr.begin(), arr.end());
    long long totalCost = 0;
    
    while (pq.size() > 1) {
        long long first = pq.top();
        pq.pop();
        long long second = pq.top();
        pq.pop();
        
        long long cost = first + second;
        totalCost += cost;
        pq.push(cost);
    }
    
    return totalCost;
}
```

## New Keywords / STL Used
`std::priority_queue`, `std::greater`

## Edge Cases
- Only 1 rope given (cost is 0).
- All ropes of same length.
