# Merge K Sorted Arrays

## Problem Statement
- given $K$ topologically independent contiguous arrays, each pre-sorted in absolute strictly ascending monotonic order, construct a mathematically unified array containing all scalar components, structurally preserving the absolute sorted geometry.


## Approach: Min-Heap Topological State Tracking

- concatenating raw vectors and sorting requires $O(N \log N)$, completely negating the geometric advantage of the pre-sorted subsets.
- to achieve mathematically optimal compression, we utilize a **Min-Heap (Priority Queue)** to execute a topological **K-Way Merge**.

- we map state tuples into the Min-Heap: `{scalar_value, array_index, element_index}`.
- initiate geometric bounds: Inject exactly the 0-th element of all $K$ valid arrays into the Min-Heap.
- because the Min-Heap universally surfaces the absolute global minimum of the current frontier, we pop the root vertex.
- append `scalar_value` to the terminal structure array.
- from the exhausted topological vector (`array_index`), we conditionally fetch the immediate subsequent element (`element_index + 1`). If mathematically valid, inject it into the Min-Heap boundary.
- the structure naturally collapses when the priority queue geometry is completely empty.


## Code Implementation

```cpp
#include <vector>
#include <queue>

using namespace std;

// Custom State Comparator topological mapping
class Compare {
public:
    bool operator()(const vector<int>& a, const vector<int>& b) {
        // Min-Heap property (Greater isolates smallest)
        return a[0] > b[0];
    }
};

vector<int> mergeKSortedArrays(vector<vector<int>>& arrays, int k) {
    // Min-Heap Structure: {value, array_idx, element_idx}
    priority_queue<vector<int>, vector<vector<int>>, Compare> min_heap;
    
    // Inject topological frontier (0-th index of K sub-arrays)
    for (int i = 0; i < k; i++) {
        if (!arrays[i].empty()) {
            min_heap.push({arrays[i][0], i, 0});
        }
    }
    
    vector<int> ans;
    
    // Iterative Boundary Collapse
    while (!min_heap.empty()) {
        auto top = min_heap.top();
        min_heap.pop();
        
        int val = top[0];
        int arr_idx = top[1];
        int ele_idx = top[2];
        
        ans.push_back(val);
        
        // Propagate spatial bounds on the specific topological vector
        if (ele_idx + 1 < arrays[arr_idx].size()) {
            min_heap.push({arrays[arr_idx][ele_idx + 1], arr_idx, ele_idx + 1});
        }
    }
    
    return ans;
}
```


## Complexity Analysis
- **time Complexity:** $O(N \log K)$ mathematically verified. Every individual scalar ($N$ total elements) is geometrically pushed and popped from the priority queue. The strict bounds of the Min-Heap are limited to exactly $K$ active elements.
- **space Complexity:** $O(K)$ bounding the explicit auxiliary storage required for the active Min-Heap state tracker. The resulting $O(N)$ geometry bounds the return matrix allocation.

> [!tip]
> **Divide and Conquer Isomorphism:** This topological construction can mathematically map equivalently utilizing a strict Divide and Conquer merging geometry (merging subsets in pairs continuously). That mapping incurs identical $O(N \log K)$ time and $O(\log K)$ call-stack space bounds. However, Heap-based K-Way Merge generally yields superior spatial cache behavior for monolithic arrays.

NEXT: [[Index]]
