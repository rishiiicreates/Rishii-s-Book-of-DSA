# Union of Two Arrays (Unsorted)

## Problem Statement
- given two unsorted arrays $A$ and $B$, evaluate their mathematical union $A \cup B$. The result must contain strictly distinct elements present in either array.


## Approach: Structural Deduplication via Hashing

- if the arrays were sorted, a [[Two Pointers]] mechanism would be mathematically optimal. For completely randomized, unsorted arrays, explicitly sorting them imposes an $\mathcal{O}(N \log N)$ penalty.
- instead, we leverage the fundamental set-theoretic property of a [[Hash Set]]: elements are intrinsically unique. By unconditionally inserting all elements from both $A$ and $B$ into a singular `std::unordered_set`, internal hash collisions map duplicates to the exact same structural memory address, effectively deduplicating them.


## Code Implementation

```cpp
#include <vector>
#include <unordered_set>

using namespace std;

vector<int> findUnion(const vector<int>& a, const vector<int>& b) {
    // Initialize Hash Set directly from Array A iterator bounds
    unordered_set<int> s(a.begin(), a.end());
    
    // Unconditionally map elements of B into the identical Set
    for (int x : b) {
        s.insert(x); // Mathematical deduplication occurs automatically
    }
    
    // Extrapolate the Set back into a continuous Vector space
    return vector<int>(s.begin(), s.end());
}
```

> [!tip]
> Because `std::unordered_set` relies on deterministic cryptographic or modular hashing, the temporal output order of the resulting vector is mathematically undefined and completely independent of the original input order.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(M + N)$ amortized. Each of the $M + N$ insertions evaluates strictly in $\mathcal{O}(1)$ average time, assuming a structurally optimal hash function mapping.
- **space Complexity:** $\mathcal{O}(M + N)$ worst case. If sets $A$ and $B$ are mathematically disjoint ($A \cap B = \emptyset$), the Hash Set will strictly contain $M + N$ distinct elements.

NEXT: [[Index]]
