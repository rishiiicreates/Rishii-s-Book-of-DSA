---
type: concept
tags: [heap, priority-queue, cpp]
date: 2026-06-30
---
# Kth Largest Element in an Array

## Problem Statement
Given an unsorted scalar array and an integer $K$, mathematically isolate and return the **$K^{th}$ largest element**. 

*(Note: It is the $K^{th}$ largest element in strictly sorted order, not the $K^{th}$ mathematically distinct element).*

---

## Approach: Min Heap (Size Constraint)

While sorting the array yields a trivial $O(N \log N)$ solution, utilizing a Priority Queue optimizes the spatial-temporal bounds significantly. 

To find the $K^{th}$ *largest* element, we utilize a **Min Heap** (which continuously exposes its minimum value at the root) constrained rigidly to size $K$.
1. Initialize an empty Min Heap.
2. Iterate through the array, sequentially inserting elements into the heap.
3. If the mathematical size of the heap strictly exceeds $K$, invoke `extractMin()`. 
   - By unconditionally discarding the absolute minimum element whenever the population exceeds $K$, the heap inherently retains only the $K$ largest elements encountered thus far.
4. Upon termination, the Min Heap contains precisely the $K$ largest elements in the array. The structural root of the Min Heap (the smallest among the largest $K$) is mathematically the $K^{th}$ largest element overall.

---

## Code Implementation

```cpp
#include <vector>
#include <queue>
using namespace std;

int findKthLargest(vector<int>& nums, int k) {
    // Topologically construct a Min Heap
    priority_queue<int, vector<int>, greater<int>> minHeap;
    
    for (int i = 0; i < nums.size(); ++i) {
        minHeap.push(nums[i]);
        
        // Strict boundary enforcement: maintain exactly K dominant elements
        if (minHeap.size() > k) {
            minHeap.pop(); // Annihilate the weakest component
        }
    }
    
    // The minimal element of the top K elements is definitively the Kth largest
    return minHeap.top();
}
```

> [!important]
> Using a **Max Heap** of size $N$ and popping $K$ times yields an $O(N + K \log N)$ solution. However, constraining a **Min Heap** to size $K$ yields an $O(N \log K)$ solution. When $K \ll N$, the Min Heap algorithm mathematically dominates.

---

## Complexity Analysis
- **Time Complexity:** $O(N \log K)$. We iterate across $N$ elements. Each element executes a heap insertion (and potential deletion), bounded by the logarithmic height of a $K$-element tree: $O(\log K)$.
- **Space Complexity:** $O(K)$. The dynamic Priority Queue never exceeds a mathematical population of $K$ elements.

*(Note: QuickSelect is an alternative algorithm achieving $O(N)$ average time complexity but degenerates to $O(N^2)$ worst-case. The Heap approach provides guaranteed asymptotic stability).*
