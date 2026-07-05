# String Subsequence Generation

## Problem Statement
- given a string $S$ of length $N$, systematically generate all $2^N$ possible contiguous or non-contiguous subsequences, preserving original temporal character order.


## Approach: Combinatorial Bitmasking

- a subsequence is mathematically formed by deciding sequentially for every character whether to *include* or *exclude* it. This creates a binary decision tree of depth $N$, yielding strictly $2^N$ terminal states.
- instead of tracking recursive paths via depth-first [[String Manipulation]], we establish a strict bijection mapping each subsequence to an integer $K \in [0, 2^N - 1]$.

- for a given string $S = s_0 s_1 \dots s_{N-1}$:
- iterate an integer mask from $0$ up to $(1 \ll N) - 1$.
- for each mask, sequentially scan bit index $j \in [0, N-1]$.
- if `mask & (1 << j)` is mathematically non-zero, the character $s_j$ is appended to the current subsequence state.
- because the inner loop traverses $j$ strictly sequentially from $0$ to $N-1$, the absolute temporal order of the extracted characters is fundamentally preserved.


## Code Implementation

```cpp
#include <vector>
#include <string>

using namespace std;

vector<string> generateSubsequences(string s) {
    int n = s.length();
    
    // Combinatorial boundary is strictly 2^n
    int limit = 1 << n;
    
    vector<string> res;
    res.reserve(limit); // Prevent dynamic reallocation overhead
    
    for (int mask = 0; mask < limit; mask++) {
        string current = "";
        current.reserve(n); // Pre-allocate memory capacity
        
        // Sequential scanning preserves temporal order
        for (int j = 0; j < n; j++) {
            // Isolate the j-th bit
            if (mask & (1 << j)) {
                current += s[j];
            }
        }
        res.push_back(current);
    }
    
    return res;
}
```

> [!tip]
> While this mathematically generates all $2^N$ subsequences, it implicitly includes the empty string `""` corresponding to the exact mask `0`. If problem bounds dictate excluding the empty subsequence, the outer loop must mathematically initialize at `int mask = 1;`.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N \cdot 2^N)$. For exactly $2^N$ integers, we linearly scan $N$ bits and conditionally append characters.
- **space Complexity:** $\mathcal{O}(N \cdot 2^N)$ to structure the result vector of strings. The temporal auxiliary space per iteration is exactly bounded by $\mathcal{O}(N)$.

NEXT: [[Index]]
