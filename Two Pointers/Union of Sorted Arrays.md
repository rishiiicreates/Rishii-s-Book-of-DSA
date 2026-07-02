---
type: concept
tags: [two-pointer, array, cpp, set-theory, math]
date: 2026-06-30
---
# Union of Two Sorted Arrays

## Problem Statement
Given two monotonic arrays $A$ and $B$, evaluate their mathematical set union $A \cup B$. The result must be strictly sorted and structurally devoid of any duplicate elements.

---

## Approach: Converging Deduplication

We iterate simultaneously through both strictly sorted arrays using a dual [[Two Pointers]] mechanism, structurally merging them similarly to Merge Sort, but enforcing strict deductive filters to guarantee set uniqueness.
1. Greedily compare $A[i]$ and $B[j]$.
2. Determine the strictly minimal theoretical candidate.
3. Validate against the ultimate element in the current result structure `res.back()`. If the minimal candidate mathematically diverges from the ultimate element, push it.
4. Increment the pointer associated with the minimal candidate. If they are exactly equal, conceptually treat it as pulling from $A$, but increment *both* pointers to bypass the structural collision.
5. After the primary iteration terminates, rigorously flush the residual elements of the unexhausted sequence using the exact same uniqueness constraint.

---

## Code Implementation

```cpp
#include <vector>

using namespace std;

vector<int> findUnion(const vector<int>& a, const vector<int>& b) {
    int n = a.size();
    int m = b.size();
    vector<int> res;
    
    int i = 0, j = 0;
    
    while (i < n && j < m) {
        if (a[i] < b[j]) {
            if (res.empty() || res.back() != a[i]) res.push_back(a[i]);
            i++;
        } 
        else if (b[j] < a[i]) {
            if (res.empty() || res.back() != b[j]) res.push_back(b[j]);
            j++;
        } 
        else {
            // Mathematical collision; resolve symmetrically
            if (res.empty() || res.back() != a[i]) res.push_back(a[i]);
            i++;
            j++;
        }
    }
    
    // Residual flushing with strict uniqueness constraint
    while (i < n) {
        if (res.empty() || res.back() != a[i]) res.push_back(a[i]);
        i++;
    }
    while (j < m) {
        if (res.empty() || res.back() != b[j]) res.push_back(b[j]);
        j++;
    }
    
    return res;
}
```

> [!important]
> Attempting to utilize `std::set` or `std::unordered_set` fundamentally violates the intrinsic structural advantage provided by the sorted inputs, degrading a theoretically optimal $\mathcal{O}(N+M)$ localized algorithm into an $\mathcal{O}((N+M) \log(N+M))$ or heavily heavily allocated $\mathcal{O}(N+M)$ amortized structure.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N + M)$. Both pointers evaluate exactly within their spatial bounds.
- **Space Complexity:** $\mathcal{O}(1)$ auxiliary space. The mathematical union inherently demands $\mathcal{O}(N + M)$ spatial structure for output.
