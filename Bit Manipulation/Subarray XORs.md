# Subarray XORs

## Problem Statement
- given an array $A$ of $N$ integers, mathematically compute the global bitwise XOR of all $\frac{N(N+1)}{2}$ contiguous subarrays.
$$ \text{Total} = \bigoplus_{i=1}^N \bigoplus_{j=i}^N \left( \bigoplus_{k=i}^j A_k \right) $$


## Approach: Frequency Parity Annihilation

- computing all subarrays explicitly takes $O(N^2)$ or $O(N^3)$ time. We can reduce this to $O(N)$ by analyzing the structural frequency of each element across all contiguous subarrays.

- for an element at 0-indexed position $i$, any contiguous subarray containing $A_i$ must have a start index $L \le i$ and an end index $R \ge i$.
- the number of valid choices for $L$ is exactly $(i + 1)$.
- the number of valid choices for $R$ is exactly $(N - i)$.
- thus, $A_i$ appears in exactly $F_i = (i + 1) \times (N - i)$ contiguous subarrays.

- because we are accumulating via the XOR operator, we invoke the Nilpotence axiom: $X \oplus X = 0$.
- if $F_i$ is **even**, $A_i$ XORs with itself an even number of times, annihilating entirely to $0$.
- if $F_i$ is **odd**, $A_i$ XORs with itself an odd number of times, mathematically collapsing to a single instance of $A_i$.

- thus, the global result is simply the XOR sum of elements where $F_i$ is odd.


## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

int subarrayBitwiseXORs(vector<int>& arr) {
    int res = 0;
    int n = arr.size();
    
    // If N is even, (i+1)*(N-i) is ALWAYS even for all i. 
    // The entire matrix collapses to 0 instantly.
    if (n % 2 == 0) return 0;
    
    for (int i = 0; i < n; i++) {
        // Frequency polynomial: F = (i + 1) * (N - i)
        long long freq = (long long)(i + 1) * (n - i);
        
        // Retain only elements with odd parities
        if (freq % 2 != 0) {
            res ^= arr[i];
        }
    }
    
    return res;
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ linear time. Or strict $O(1)$ absolute if $N$ is even!
- **space Complexity:** $O(1)$ auxiliary space.

> [!tip]
> **Even $N$ Annihilation Theorem:** Observe the polynomial $F_i = (i + 1) \times (N - i)$. If $N$ is even, exactly one of the terms $(i+1)$ or $(N-i)$ must be even, meaning $F_i$ is strictly even for ALL $i$. Thus, if the array length $N$ is even, the XOR sum of all subarrays is mathematically guaranteed to be $0$ without reading a single array element!

NEXT: [[Index]]
