# Is Subset Verification

## Problem Statement
- given two arrays $A$ and $B$, evaluate mathematically whether $A \subseteq B$ (i.e., every element present in $A$ is also present in $B$). The arrays may contain structural duplicates. If $A$ contains duplicates of an element $x$, the strict mathematical formulation simply requires that $x \in B$, regardless of multiplicity.


## Approach: Constant-Time Hashing Lookups

- determining subset inclusion traditionally requires $\mathcal{O}(M \times N)$ time via nested iterative scanning. By structurally mapping array $B$ into an auxiliary [[Hash Set]], we reduce the membership evaluation $x \in B$ to an $\mathcal{O}(1)$ amortized lookup.

- algorithm:
- pre-process array $B$ by allocating all its elements into a `std::unordered_set`. This maps the values to a hashed memory layout, structurally eliminating duplicates.
- iteratively traverse array $A$.
- for every element $x \in A$, query the Hash Set. If the query strictly fails to locate $x$ (`setB.find(x) == setB.end()`), $A \not\subseteq B$. Return `false`.
- if the iteration exhausts $A$ without encountering any failures, the mathematical proposition $A \subseteq B$ holds true.


## Code Implementation

```cpp
#include <vector>
#include <unordered_set>

using namespace std;

bool isSubset(const vector<int>& A, const vector<int>& B) {
    // Structural mapping of B into a Hash Set
    // This executes N insertion operations bounded by O(1) amortized
    unordered_set<int> setB(B.begin(), B.end());
    
    // Evaluate proposition: For all x in A, x exists in B
    for (int x : A) {
        if (setB.find(x) == setB.end()) {
            return false; // Mathematical violation detected
        }
    }
    
    return true; // Proposition holds
}
```

> [!important]
> If the problem defines subset inclusion such that multiplicity matters (e.g., if $A$ contains two $5$s, $B$ must contain at least two $5$s), a `std::unordered_set` fundamentally fails as it strips frequency data. In that specific rigorous variant, a `std::unordered_map` (frequency map) is mathematically required.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(M + N)$ amortized, where $M = |A|$ and $N = |B|$. The constructor iterates $N$ elements, and the evaluation iterates $M$ elements.
- **space Complexity:** $\mathcal{O}(N)$ to store the structural bounds of $B$ in the hash table.

NEXT: [[Index]]
