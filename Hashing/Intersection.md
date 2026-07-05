# Intersection of Two Arrays (Unsorted)

## Problem Statement
- given two unsorted arrays $A$ and $B$, evaluate their mathematical intersection $A \cap B$. The intersection must contain strictly distinct elements that physically reside in both original structures.


## Approach: Bipartite Hash Mapping

- for unsorted arrays, we map one array into a [[Hash Set]] to enable $\mathcal{O}(1)$ amortized lookups.
- map every element of $A$ into a structural `std::unordered_set`.
- linearly traverse $B$. For each element $x \in B$, query the Set.
- if $x$ exists in the Set, it fundamentally belongs to $A \cap B$. Push it to the result structure.
- **critical Deduplication Step:** Immediately execute `s.erase(x)` from the Set. If array $B$ contains duplicate instances of $x$, subsequent queries for $x$ will mathematically fail, strictly preserving uniqueness in the final intersection vector.


## Code Implementation

```cpp
#include <vector>
#include <unordered_set>

using namespace std;

vector<int> intersection(const vector<int>& a, const vector<int>& b) {
    // Map bounds of A into Hash space
    unordered_set<int> s(a.begin(), a.end());
    
    vector<int> res;
    
    // Evaluate intersections across B
    for (int x : b) {
        // Query presence
        if (s.find(x) != s.end()) {
            res.push_back(x);
            // Destructively erase to guarantee set uniqueness
            s.erase(x);
        }
    }
    
    return res;
}
```

> [!important]
> As an absolute optimization rule, you should always map the **smaller** array into the Hash Set. If $|A| > |B|$, passing $B$ into the constructor strictly bounds the auxiliary spatial complexity to $\mathcal{O}(\min(M, N))$ rather than $\mathcal{O}(\max(M, N))$.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(M + N)$ amortized time.
- **space Complexity:** $\mathcal{O}(M)$ strictly to map the array $A$ into the associative hash structure. Output array space is excluded from auxiliary boundaries.

NEXT: [[Index]]
