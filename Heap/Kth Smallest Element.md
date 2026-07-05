# Kth Smallest Element in an Array

## Problem Statement
- given an unsorted scalar array and an integer $K$, mathematically isolate and return the **$K^{th}$ smallest element**.

- *(note: It is the $K^{th}$ smallest element in strictly sorted order, not the $K^{th}$ mathematically distinct element).*


## Approach: Max Heap (Size Constraint)

- this is the exact topological inversion of the [[Kth Largest Element]] algorithm. To find the $K^{th}$ *smallest* element efficiently, we employ a **Max Heap** constrained rigidly to size $K$.

- initialize an empty Max Heap.
- iterate through the array, inserting elements into the heap.
- if the mathematical size of the heap strictly exceeds $K$, invoke `extractMax()`.
   - by purging the absolute maximum element whenever the population exceeds $K$, the heap continuously filters out large values, permanently retaining only the $K$ smallest elements encountered.
- upon termination, the Max Heap contains exactly the $K$ smallest elements. The root (the largest among the smallest $K$) is mathematically the $K^{th}$ smallest element.


## Code Implementation

```cpp
#include <vector>
#include <queue>
using namespace std;

int findKthSmallest(vector<int>& nums, int k) {
    // Topologically construct a Max Heap (C++ Default)
    priority_queue<int> maxHeap;
    
    for (int i = 0; i < nums.size(); ++i) {
        maxHeap.push(nums[i]);
        
        // Strict boundary enforcement: maintain exactly K subordinate elements
        if (maxHeap.size() > k) {
            maxHeap.pop(); // Annihilate the dominant component
        }
    }
    
    // The maximal element of the bottom K elements is definitively the Kth smallest
    return maxHeap.top();
}
```

> [!warning]
> Do not attempt to load all $N$ elements into a Min Heap and pop $K$ times. While syntactically simple, it consumes $O(N)$ auxiliary space and forces the time complexity up to $O(N + K \log N)$. Constraining a Max Heap to size $K$ is the mathematically rigorous approach for stream processing and massive datasets.


## Complexity Analysis
- **time Complexity:** $O(N \log K)$. Every element undergoes heap operations bounded by the height of a $K$-element heap, executing in strictly $O(\log K)$ time.
- **space Complexity:** $O(K)$. The internal data structure dynamically allocates memory strictly bounded to $K$ scalar integers.

NEXT: [[Index]]
