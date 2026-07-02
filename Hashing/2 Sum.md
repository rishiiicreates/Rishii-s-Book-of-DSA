---
type: concept
tags: [hashing, cpp, two-pointers, math]
date: 2026-06-30
---
# 2 Sum (Unsorted Variant)

## Problem Statement
Given an unsorted array of integers and an integer `target`, mathematically verify if there exist exactly two elements $x$ and $y$ such that $x + y = \text{target}$.

---

## Approach: Temporal State Hashing

The brute-force $\mathcal{O}(N^2)$ method evaluates every unique combinatorial pair. For a linear $\mathcal{O}(N)$ approach on an unsorted array, we leverage the associative properties of algebra.
If we are currently inspecting an element $x$, the mathematical condition for success requires finding a previously seen element $y$ such that $y = \text{target} - x$.

Algorithm:
1. Initialize an empty [[Hash Set]] to strictly log "elements mathematically observed in the past".
2. Iterate $x$ through the array.
3. Dynamically evaluate the complement: $C = \text{target} - x$.
4. Query the Hash Set for $C$. If found, the pair $(x, C)$ structurally exists. Return `true`.
5. If not found, insert $x$ into the Hash Set for future temporal evaluations.

---

## Code Implementation

```cpp
#include <vector>
#include <unordered_set>

using namespace std;

bool hasTwoSum(const vector<int>& arr, int target) {
    unordered_set<int> seen;
    
    for (int x : arr) {
        // Algebraic complement deduction
        int complement = target - x;
        
        // Evaluate historical temporal state
        if (seen.find(complement) != seen.end()) {
            return true;
        }
        
        // Push current state into historical record
        seen.insert(x);
    }
    
    return false; // Mathematical impossibility
}
```

> [!tip]
> A common structural error is inserting every element of `arr` into the Hash Set *before* iterating. That fundamentally fails if `target = 10` and `arr = [5, 2]`. The array only has one `5`, but `10 - 5 = 5`, which the pre-loaded Set will falsely flag as an independently valid element. Dynamically inserting elements *after* the query completely neutralizes this flaw.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$ amortized. The linear scan executes strictly $N$ iterations, each demanding an $\mathcal{O}(1)$ average hash table query.
- **Space Complexity:** $\mathcal{O}(N)$ worst-case auxiliary space to cache the temporal history.
