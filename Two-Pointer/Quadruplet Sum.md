# Quadruplet Sum (Hash Map Approach)

## Problem Statement
- evaluate whether there exists exactly four unique elements within an array of size $N$ that algebraically sum to a specific integer $T$.


## Approach: Bipartite Combinatorial Hashing

- while the strictly sorted [[Two-Pointer]] approach guarantees $\mathcal{O}(N^3)$ bounds, determining just the *existence* of a quadruplet (without requiring lexicographical uniqueness of all valid sets) can theoretically be resolved in $\mathcal{O}(N^2)$ average time using a paired [[Hash Map]].

- algorithm:
- geometrically bisect the equation $a + b + c + d = T$ into $(a + b) = T - (c + d)$.
- we iterate through the array evaluating the sum of pairs $(a, b)$ and mapping their algebraic sum to their respective indices in a Hash Map.
- crucially, to prevent temporal crossover (using the exact same index twice), we only map $(a, b)$ pairs that geometrically precede the currently evaluated $(c, d)$ pair.
- for every element $nums[i]$ and $nums[j]$ (where $i < j$), we evaluate the complement $C = T - (nums[i] + nums[j])$. If $C$ mathematically exists in the Hash Map, a distinct quadruplet is structurally guaranteed.


## Code Implementation

```cpp
#include <vector>
#include <unordered_map>
#include <utility>
#include <climits>

using namespace std;

bool hasQuadrupletSum(const vector<int>& nums, int target) {
    int n = nums.size();
    if (n < 4) return false;
    
    // Hash Map tracking Pair Sum -> indices
    unordered_map<long long, pair<int, int>> pairSumMap;
    
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            // Evaluate algebraic complement
            long long currentSum = 1LL * nums[i] + nums[j];
            long long complement = 1LL * target - currentSum;
            
            // Query existence in historical Hash Map
            if (pairSumMap.find(complement) != pairSumMap.end()) {
                return true; // Valid disjoint quadruplet mathematically exists
            }
        }
        
        // Push historical pairs strictly bounding at i
        // This ensures no spatial overlap between (a, b) and (c, d)
        for (int k = 0; k < i; k++) {
            long long pastSum = 1LL * nums[k] + nums[i];
            pairSumMap[pastSum] = {k, i};
        }
    }
    
    return false;
}
```

> [!important]
> The exact temporal placement of `pairSumMap` insertion is mathematically critical. If you pre-calculate all $\mathcal{O}(N^2)$ pairs into the map *before* querying, you risk falsely validating a quadruplet constructed from overlapping structural indices (e.g., using `nums[3]` twice). Interleaving the map accumulation strictly *behind* the evaluation frontier guarantees mathematical disjointness.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N^2)$ amortized. Evaluating $N^2$ states with an $\mathcal{O}(1)$ average table lookup.
- **space Complexity:** $\mathcal{O}(N^2)$ to store the algebraic pairs in the Hash Map.

NEXT: [[Index]]
