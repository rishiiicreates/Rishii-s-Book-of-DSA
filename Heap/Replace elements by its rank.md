# Replace Elements by its Rank in the Array

## Problem Statement
- given an array $A$ of $N$ integers, mathematically map each element to its absolute ordinal rank. The rank sequence must satisfy:
- the absolute minimum scalar in the array mathematically receives a rank of 1.
- if two elements map to strict equality ($A[i] = A[j]$), they must share the identical topological rank.
- the absolute rank increments strictly monotonically by 1 ($\text{Rank}_{k} = \text{Rank}_{k-1} + 1$) for sequentially larger unique scalars.
- the output array $R$ must structurally mirror the topological geometry of the input array $A$ such that $R[i] = \text{Rank}(A[i])$.


## Approach: Sorting and Hash Map Binding

- to establish a strict monotonic ranking, we require a topologically sorted extraction of unique scalars.
- we can utilize a **Min-Heap (Priority Queue)** to iteratively extract scalars in strictly ascending order, but a pure $O(N \log N)$ Array Sorting mechanism coupled with a Hash Map is structurally superior due to lower auxiliary overhead and improved cache locality.

- construct a topological clone of $A$ and sort it in strict monotonic ascending order.
- iterate the sorted boundary array. If the current scalar does not mathematically exist in the Hash Map, bind it to the current absolute `rank` and mutate `rank++`.
- geometrically iterate the original array $A$, substituting $A[i]$ with its mapped constant time $O(1)$ rank lookup.


## Code Implementation

```cpp
#include <vector>
#include <unordered_map>
#include <algorithm>

using namespace std;

vector<int> replaceWithRank(vector<int>& arr) {
    int n = arr.size();
    if (n == 0) return {};
    
    // Topological copy for monotonic evaluation
    vector<int> sorted_arr = arr;
    sort(sorted_arr.begin(), sorted_arr.end());
    
    // Hash state map: Scalar -> Rank
    unordered_map<int, int> rank_map;
    int current_rank = 1;
    
    // Geometrically bind unique scalars
    for (int i = 0; i < n; i++) {
        if (rank_map.find(sorted_arr[i]) == rank_map.end()) {
            rank_map[sorted_arr[i]] = current_rank++;
        }
    }
    
    // Construct ordinal geometry
    vector<int> ans(n);
    for (int i = 0; i < n; i++) {
        ans[i] = rank_map[arr[i]];
    }
    
    return ans;
}
```


## Complexity Analysis
- **time Complexity:** $O(N \log N)$ bounded strictly by the topological sorting algorithm. Hash map insertions and lookups evaluate to amortized $O(1)$.
- **space Complexity:** $O(N)$ bounding the structural storage of the sorted array clone and the auxiliary Hash Map geometry.

> [!tip]
> **Priority Queue Isomorphism:** While a Min-Heap can structurally achieve this by pushing all elements and popping them iteratively to map ranks, it incurs identical $O(N \log N)$ time and $O(N)$ space but suffers from heavier constant overhead due to sequential heapify down operations versus contiguous array sorts.

NEXT: [[Index]]
