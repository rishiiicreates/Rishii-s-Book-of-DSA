---
type: concept
tags: [heap, priority-queue, sorting, cpp]
date: 2026-06-30
---
# Sort a K-Sorted Array (Nearly Sorted Array)

## Problem Statement
Given an array of $N$ elements, where each element is at most $K$ topological positions away from its target position in the perfectly sorted array, mathematically sort the entire array in absolute minimal time.

*Example Constraints:* If $K=2$, an element destined for index 3 could theoretically be found at indices 1, 2, 3, 4, or 5.

---

## Approach: Sliding Min-Heap Window

A naive sort computes in $O(N \log N)$. However, the constraint $K$ provides a strict mathematical boundary that we can exploit. The absolute minimum element of the entire array mathematically *must* exist within the first $K+1$ elements (indices $0$ to $K$). 

We can establish a sliding topological window utilizing a **Min Heap** of size $K+1$.
1. **Window Initialization:** Load the first $K+1$ elements of the array into a Min Heap.
2. **Sequential Finalization:** 
   - The root of the Min Heap (the absolute minimum of the current window) is mathematically guaranteed to be the absolute minimum of all remaining unsorted elements.
   - Pop this element and permanently lock it into the output array.
   - Insert the next available element from the input array into the heap.
3. **Flushing:** Once the input array is fully consumed, sequentially pop and place the remaining $K$ elements from the heap into the output array.

---

## Code Implementation

```cpp
#include <vector>
#include <queue>
using namespace std;

void sortKSortedArray(vector<int>& arr, int k) {
    int n = arr.size();
    if (n == 0) return;
    
    // Topologically constrained Min Heap
    priority_queue<int, vector<int>, greater<int>> minHeap;
    
    // Initialize the K-bounded sliding window
    // Bounded by n to prevent buffer overflows if K > N
    int limit = min(k + 1, n);
    for (int i = 0; i < limit; ++i) {
        minHeap.push(arr[i]);
    }
    
    int target_index = 0;
    
    // Process the dynamic sliding window
    for (int i = k + 1; i < n; ++i) {
        arr[target_index++] = minHeap.top(); // Mathematically stable minimal element
        minHeap.pop();
        minHeap.push(arr[i]);
    }
    
    // Exhaust the remaining structural buffer
    while (!minHeap.empty()) {
        arr[target_index++] = minHeap.top();
        minHeap.pop();
    }
}
```

> [!important]
> The space complexity of this algorithm is profoundly optimal for massive data streams (e.g., $N = 10^9$, $K = 100$). The algorithm only requires a $100$-element memory footprint to sort a billion-element stream dynamically. 

---

## Complexity Analysis
- **Time Complexity:** $O(N \log K)$. Initializing the heap takes $O(K)$. Processing the remaining $N-K$ elements incurs an extraction and insertion cost of $O(\log K)$ per element. Flushing the heap costs $O(K \log K)$. This sums to an absolute asymptotic boundary of $O(N \log K)$.
- **Space Complexity:** $O(K)$ auxiliary space. The Min Heap is strictly constrained to a maximum population of $K+1$ elements.
