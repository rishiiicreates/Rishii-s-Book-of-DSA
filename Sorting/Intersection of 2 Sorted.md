---
type: concept
tags: [sorting, cpp, two-pointers, set-theory]
date: 2026-06-30
---
# Intersection of Two Sorted Arrays

## Problem Statement
Given two sorted arrays $A$ and $B$, evaluate their mathematical intersection $A \cap B$. The intersection should contain exactly one instance of each distinct common element.

---

## Approach: Two-Pointer Convergence with Deduplication

Since the arrays are strictly sorted, we evaluate the intersection iteratively via a synchronized [[Two Pointers]] mechanism without any supplementary Hash Maps.
1. Maintain pointers $i$ for $A$ and $j$ for $B$.
2. If $A[i] < B[j]$, it is mathematically impossible for $A[i]$ to exist in the remaining unseen elements of $B$, since $B$ is monotonically ascending. Thus, increment $i$.
3. If $B[j] < A[i]$, symmetrically increment $j$.
4. If $A[i] == B[j]$, we have discovered a common element in the intersection. Push it to `res` (subject to deduplication via `res.back()`) and increment both $i$ and $j$.
5. The algorithm terminates immediately when *either* pointer violates bounds, as intersection requires dual existence.

---

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
        if (a[i] < b[j]) {
            i++;
        } 
        else if (b[j] < a[i]) {
            j++;
        } 
        else { // Mathematical intersection equality
            if (res.empty() || res.back() != a[i]) {
                res.push_back(a[i]);
            }
            i++;
            j++;
        }
    }
    
    return res;
}
```

> [!tip]
> Unlike the Union algorithm, Intersection does not require flushing remaining elements. The moment one array is mathematically exhausted, no further intersections can logically occur.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N + M)$. In the absolute worst case, we iteratively advance one pointer at a time until one array is fully traversed.
- **Space Complexity:** $\mathcal{O}(\min(N, M))$ to store the output array. The intersection can logically never exceed the size of the smaller bounding set. Auxiliary space is strictly $\mathcal{O}(1)$.
