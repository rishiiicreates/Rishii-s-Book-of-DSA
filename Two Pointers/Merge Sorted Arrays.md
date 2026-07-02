---
type: concept
tags: [two-pointer, array, cpp, sorting, math]
date: 2026-06-30
---
# Merge Two Sorted Arrays

## Problem Statement
Given two strictly sorted arrays $A$ and $B$, evaluate their mathematical combination into a single strictly sorted array.

---

## Approach: Two-Pointer Monotonic Convergence

Because the sequences are fundamentally monotonic, we can bypass general $\mathcal{O}((N+M) \log(N+M))$ sorting algorithms and merge them linearly in $\mathcal{O}(N+M)$.
We deploy a dual [[Two-Pointer]] convergence model:
1. Initialize pointer $i = 0$ for array $A$ and $j = 0$ for array $B$.
2. Greedily compare $A[i]$ and $B[j]$.
3. Extract the strictly smaller mathematical element and push it to the output sequence, incrementing its corresponding pointer.
4. If $A[i] = B[j]$, mathematical stability dictates evaluating either arbitrarily (though structurally, pulling from $A$ preserves relative sequence stability).
5. Once one boundary exhausts, forcefully flush the residual elements of the remaining unexhausted array.

---

## Code Implementation

```cpp
#include <vector>

using namespace std;

vector<int> mergeArrays(const vector<int>& a, const vector<int>& b) {
    int n = a.size();
    int m = b.size();
    vector<int> res;
    res.reserve(n + m); // Mathematical pre-allocation to prevent dynamic structural reallocation
    
    int i = 0;
    int j = 0;
    
    while (i < n && j < m) {
        if (a[i] <= b[j]) {
            res.push_back(a[i++]); // Extracts and advances
        } else {
            res.push_back(b[j++]);
        }
    }
    
    // Asymmetric sequence flushing
    while (i < n) res.push_back(a[i++]);
    while (j < m) res.push_back(b[j++]);
    
    return res;
}
```

> [!important]
> For variants requiring **in-place** merging (e.g., LeetCode 88, where $A$ has excess padded capacity), the pointers must structurally evaluate from the **rear** ($N-1$ and $M-1$) downwards. A forward evaluation mathematically overwrites unread states in $A$.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N + M)$. Both sequences are evaluated exactly once.
- **Space Complexity:** $\mathcal{O}(N + M)$ to structure the returned array. The temporal auxiliary space is strictly $\mathcal{O}(1)$.
