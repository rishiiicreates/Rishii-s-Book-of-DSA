# Union of Two Sorted Arrays

## Problem Statement
- given two sorted arrays $A$ and $B$, evaluate their mathematical union $A \cup B$. The resulting array must be strictly sorted and contain exactly one instance of each distinct element present in either $A$ or $B$.


## Approach: Two-Pointer Convergence with Deduplication

- since both arrays are strictly sorted, we can avoid evaluating a Hash Set ($\mathcal{O}(N+M)$ space) and simply traverse both arrays simultaneously using a [[Two Pointers]] mechanism.
- maintain pointers $i$ for $A$ and $j$ for $B$.
- in each iteration, greedily compare $A[i]$ and $B[j]$.
- push the strictly smaller value into the result array `res`, and increment the corresponding pointer. If they are exactly equal, push one instance and increment **both** pointers.
- crucially, before pushing any candidate value to `res`, we must assert a duplication check: `if (res.empty() || res.back() != candidate)`. This guarantees strict set uniqueness.
- after the primary loop terminates, flush any remaining elements from $A$ or $B$, maintaining the same deduplication logic.


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
        // Evaluate the strictly smaller candidate
        if (a[i] < b[j]) {
            if (res.empty() || res.back() != a[i]) {
                res.push_back(a[i]);
            }
            i++;
        } 
        else if (b[j] < a[i]) {
            if (res.empty() || res.back() != b[j]) {
                res.push_back(b[j]);
            }
            j++;
        } 
        else { // Mathematical equality
            if (res.empty() || res.back() != a[i]) {
                res.push_back(a[i]);
            }
            i++;
            j++;
        }
    }
    
    // Flush the remaining exhausted arrays
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
> The deduplication check `res.back() != candidate` is absolutely essential. It cleanly collapses internal duplicates within $A$, internal duplicates within $B$, and cross-duplicates between $A$ and $B$, all using $\mathcal{O}(1)$ localized logic.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N + M)$. Both pointers monotonically advance across the strictly bounded bounds of the respective arrays.
- **space Complexity:** $\mathcal{O}(N + M)$ to store the mathematical union in the output array. Auxiliary space is strictly $\mathcal{O}(1)$.

NEXT: [[Index]]
