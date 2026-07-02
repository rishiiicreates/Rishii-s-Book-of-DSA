---
type: concept
tags: [sorting, cpp, graph]
date: 2026-06-30
---
# Minimum Swaps to Sort

## Problem Statement
Given an array $A$ of $N$ distinct elements, find the minimum number of swaps required to sort the array in strictly ascending order.

*Example:* $A = [2, 8, 5, 4]$
*Result:* $1$ (Swap $8$ and $4$ to get $[2, 4, 5, 8]$)

---

## Approach: Graph Theory (Cycle Decomposition)

The problem reduces to decomposing a permutation into disjoint cycles. 
1. Attach the original index to each element and sort the array by values. This reveals where each element *should* go.
2. The sorted array tells us: the element originally at index $i$ must move to index $j$. This defines a directed edge $i \to j$.
3. The graph decomposes into a set of disjoint cycles.
4. For a cycle of length $L$, mathematically it requires exactly $L - 1$ swaps to resolve.
5. So, the total number of swaps is $\sum (\text{Cycle\_Length} - 1)$.
6. We can trace cycles directly: iterate through the sorted pairs. If an element is already in the correct place, ignore it. If not, swap it continuously with the element occupying its original index until the cycle closes.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

int minSwaps(vector<int>& nums) {
    int n = nums.size();
    
    // Store pairs of (value, original_index)
    vector<pair<int, int>> arr(n);
    for (int i = 0; i < n; ++i) {
        arr[i] = {nums[i], i};
    }
    
    // Sort to determine the correct target positions
    sort(arr.begin(), arr.end());
    
    int swaps = 0;
    
    // Traverse the array to resolve cycles
    for (int i = 0; i < n; ++i) {
        // If element is already at the correct original index, cycle is resolved
        if (arr[i].second == i) {
            continue;
        }
        
        // Otherwise, swap it to its correct target index
        swap(arr[i], arr[arr[i].second]);
        swaps++;
        
        // Decrement i to re-check the new element swapped into position i
        i--;
    }
    
    return swaps;
}
```

> [!tip]
> A mathematically equivalent implementation uses a `visited` boolean array. You follow the $i \to \text{target}$ edges to trace the cycle, marking nodes as `visited`. If a cycle has length $L$, you add $L - 1$ to the total swap count. The swap-in-place method shown above achieves the same result elegantly without a `visited` array.

---

## Complexity Analysis
- **Time Complexity:** $O(N \log N)$. Sorting the pairs dominates the execution time. Resolving the cycles via swaps takes exactly $O(N)$ time since each element is swapped at most once to its final position.
- **Space Complexity:** $O(N)$ to store the pairs of values and original indices.
