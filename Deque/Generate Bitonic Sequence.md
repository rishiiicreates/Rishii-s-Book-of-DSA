# Generate Bitonic Sequence

## Problem Statement
- given an array sequence $A$ of $N$ discrete integers, construct a mathematically pure **Bitonic Sequence** using all scalars. A sequence is structurally Bitonic if it is geometrically bounded by a single global peak—specifically, it monotonically increases to a maximum scalar limit, and then strictly monotonically decreases.


## Approach: Bidirectional Boundary Construction

- to guarantee a geometric Bitonic topology, the mathematically minimal elements must structurally reside at the extreme spatial edges of the output sequence, whilst the absolute maximum scalar must map directly to the topological geometric center.

- we sort the sequence $A$ in strictly monotonically non-decreasing order.
- we construct the output sequence by iterating the sorted array and alternatively mapping elements to the spatial boundaries:
- sort sequence $A$.
- initialize an empty Deque $D$.
- for each sequentially processed scalar $x_i \in A$:
   - if index $i$ is geometrically Even: `push_back(x_i)`
   - if index $i$ is geometrically Odd: `push_front(x_i)`

- this dual-boundary algorithm injects the smallest scalars to the structural edges, and mathematically funnels the largest terminal elements directly to the topological center, perfectly axiomatizing a symmetric Bitonic geometry.


## Code Implementation

```cpp
#include <vector>
#include <deque>
#include <algorithm>

using namespace std;

vector<int> generateBitonicSequence(vector<int>& A) {
    // Topologically order absolute scalar magnitude
    sort(A.begin(), A.end());
    
    deque<int> dq;
    
    // Map elements geometrically using parity constraints
    for (int i = 0; i < A.size(); i++) {
        if (i % 2 == 0) {
            dq.push_back(A[i]);
        } else {
            dq.push_front(A[i]);
        }
    }
    
    // Collapse topological geometry to flat sequence
    return vector<int>(dq.begin(), dq.end());
}
```


## Complexity Analysis
- **time Complexity:** $O(N \log N)$ mandated strictly by the bounds of comparison-based sorting topology. The bidirectional sequence mapping operates in exact $O(N)$ linear bound.
- **space Complexity:** $O(N)$ auxiliary geometric state holding the final sequence structure.

> [!warning]
> **Strictness and Monotonicity Limits:** The problem definition usually assumes all integers in $A$ are distinct. If scalar duplicates exist, they map to adjacent topological positions in the Deque, altering the geometry from strictly monotonically increasing/decreasing to weakly non-decreasing/non-increasing. The bitonic topology remains fundamentally valid under weak monotonic parameters.

NEXT: [[Index]]
