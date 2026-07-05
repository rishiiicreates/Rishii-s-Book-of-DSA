# Subarray Sum Equals K

## Problem Statement
- given an array of mathematical scalars (which may be negative) and a strict target sequence sum $K$, evaluate the exact absolute count of contiguous spatial subarrays that structurally evaluate to $K$.


## Approach: Prefix Sum Algebraic Complements

- while dynamic sliding windows ([[Two Pointers]]) solve this optimally for strictly non-negative sequences, the presence of negative scalars fundamentally destroys the monotonic expansion constraint (adding a number no longer guarantees an increased sum).
- we must shift to algebraic [[Prefix Sum]] analysis.

- let $S_i$ denote the mathematical sum from index $0$ to $i$. The sum of an arbitrary spatial subarray spanning indices $[j, i]$ (where $j \le i$) is evaluated via structural difference:
$$ \text{Sum}(j, i) = S_i - S_{j-1} $$

- we mathematically require $\text{Sum}(j, i) = K$.
$$ S_i - S_{j-1} = K \implies S_{j-1} = S_i - K $$

- algorithm:
- iteratively accumulate the prefix sum $S_i$.
- for each iteration, compute the mathematical complement: $C = S_i - K$.
- query a Hash Map (`unordered_map<int, int>`) tracking the strict absolute frequencies of all historically encountered prefix sums. If $C$ exists in the map, add its structural frequency to our global valid subarray count.
- finally, record the current spatial prefix sum $S_i$ into the Hash Map.


## Code Implementation

```cpp
#include <vector>
#include <unordered_map>

using namespace std;

int subarraySum(const vector<int>& nums, int k) {
    // Spatial mapping: Prefix Sum -> Frequency
    unordered_map<long long, int> prefix_counts;
    
    // Mathematical initialization: Prefix sum of 0 occurs exactly once initially
    prefix_counts[0] = 1;
    
    long long current_sum = 0;
    int valid_subarrays = 0;
    
    for (int x : nums) {
        // Accumulate temporal spatial sequence
        current_sum += x;
        
        // Evaluate algebraic complement
        long long complement = current_sum - k;
        
        // Accumulate structural frequencies of matching historical states
        if (prefix_counts.find(complement) != prefix_counts.end()) {
            valid_subarrays += prefix_counts[complement];
        }
        
        // Push current temporal state into historical bounds
        prefix_counts[current_sum]++;
    }
    
    return valid_subarrays;
}
```

> [!warning]
> The absolute requirement to initialize `prefix_counts[0] = 1` evaluates the critical edge case where the strictly bounded sequence spanning from index $0$ perfectly equals $K$ (i.e., $S_{j-1}$ is theoretically non-existent or logically evaluates to a sum of $0$).


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$ amortized. A strict linear progression through $N$ elements executes $\mathcal{O}(1)$ average theoretical lookups against the Hash Map.
- **space Complexity:** $\mathcal{O}(N)$ bounded auxiliary space to map exactly $N$ distinct prefix evaluations within the Hash Map.

NEXT: [[Index]]
