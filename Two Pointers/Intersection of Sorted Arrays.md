# Intersection of Two Sorted Arrays

## Problem Statement
- given two strictly monotonic (sorted) arrays $A$ and $B$, geometrically evaluate their mathematical intersection $A \cap B$. The evaluated intersection must inherently maintain sorting and contain only strictly unique instances of the common elements.


## Approach: Synchronous Dual Pointers

- because the structures are strictly sorted, we completely bypass auxiliary Hash Maps and utilize synchronized [[Two Pointers]].
- initialize $i = 0$ for $A$ and $j = 0$ for $B$.
- if $A[i] < B[j]$, it is mathematically impossible for $A[i]$ to exist in the remaining unseen elements of $B$, since $B$ is monotonically ascending. Thus, increment $i$.
- if $B[j] < A[i]$, symmetrically increment $j$.
- if $A[i] == B[j]$, a structural intersection exists. Push it to the result array (subject to mathematical deduplication via `res.back()`) and sequentially increment both $i$ and $j$.
- the algorithm terminates immediately when *either* pointer violates bounds.


## Code Implementation

```cpp
#include <vector>

using namespace std;

vector<int> findIntersection(const vector<int>& a, const vector<int>& b) {
    int n = a.size();
    int m = b.size();
    vector<int> res;
    
    int i = 0, j = 0;
    
    while (i < n && j < m) {
        // Geometric divergence logic
        if (a[i] < b[j]) {
            i++;
        } 
        else if (b[j] < a[i]) {
            j++;
        } 
        else { 
            // Structural equivalence
            if (res.empty() || res.back() != a[i]) {
                res.push_back(a[i]); // Record unique intersection
            }
            i++;
            j++;
        }
    }
    
    return res;
}
```

> [!tip]
> Unlike mathematical Unions, Intersections structurally terminate the precise moment either individual array is exhausted. No residual sequence flushing is theoretically required.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N + M)$. In the absolute worst case, pointers independently increment up to the bounds.
- **space Complexity:** $\mathcal{O}(1)$ auxiliary structural memory. The return structure demands $\mathcal{O}(\min(N, M))$ bounds.

NEXT: [[Index]]
