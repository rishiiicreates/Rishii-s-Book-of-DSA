# Power Set Generation via Bitmasking

## Problem Statement
- given an array `nums` of $N$ strictly unique integers, mathematically evaluate and return the Power Set (the set of all possible subsets).


## Approach: Combinatorial Bitmasking

- according to set theory combinatorics, a set of cardinality $N$ contains exactly $2^N$ distinct subsets.
- traditionally, subsets are generated via [[Backtracking]] recursion. However, we can establish a strict mathematical bijection between the subsets of `nums` and the integers in the bounded range $[0, 2^N - 1]$.

- for any integer $K \in [0, 2^N - 1]$:
- express $K$ in its $N$-bit binary representation.
- if the $j$-th bit of $K$ is set to $1$, the element `nums[j]` is strictly included in the corresponding subset.
- if the $j$-th bit is $0$, the element is excluded.

- by linearly iterating $K$ from $0$ to $2^N - 1$, we systematically traverse every possible binary configuration, deterministically yielding the entire Power Set without any recursive stack overhead.


## Code Implementation

```cpp
#include <vector>

using namespace std;

vector<vector<int>> subsets(vector<int>& nums) {
    int n = nums.size();
    
    // Total subset count is strictly 2^n
    // Using 1 << n mathematically evaluates 2^n
    int subset_count = 1 << n;
    
    vector<vector<int>> res;
    res.reserve(subset_count); // Memory pre-allocation to prevent reallocations
    
    // Linearly sweep the combinatorial space
    for (int mask = 0; mask < subset_count; mask++) {
        vector<int> current;
        // The maximum subset size is mathematically bounded by n
        current.reserve(n); 
        
        for (int j = 0; j < n; j++) {
            // Isolate the j-th bit to evaluate inclusion
            if (mask & (1 << j)) {
                current.push_back(nums[j]);
            }
        }
        res.push_back(current);
    }
    
    return res;
}
```

> [!warning]
> The strict mathematical bounds dictate that $N$ cannot exceed $31$ for standard 32-bit signed integers. Since `1 << 31` overflows into negative domains in signed arithmetic, an array size $N > 30$ demands `1LL << N` and `long long` masks. However, generating $2^{31}$ subsets explicitly exhausts physical RAM limitations, restricting $N \le 20$ in practical evaluation systems.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N \cdot 2^N)$. There are strictly $2^N$ masks, and processing each mask demands an inner loop evaluating exactly $N$ bits.
- **space Complexity:** $\mathcal{O}(N \cdot 2^N)$ to accumulate and store the output Power Set structure. Auxiliary space is strictly $\mathcal{O}(N)$ for the dynamic `current` vector.

NEXT: [[Index]]
