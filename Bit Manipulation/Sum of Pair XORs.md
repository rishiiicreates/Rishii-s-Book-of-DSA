# Sum of Pair XORs

## Problem Statement
- given an array $A$ of $N$ integers, mathematically compute the total arithmetic sum of the XOR of all combinatorial pairs.
$$ \text{Total} = \sum_{i=1}^{N-1} \sum_{j=i+1}^N (A_i \oplus A_j) $$


## Approach: Bitwise Combinatorial Parity

- a naive double loop generates all $\binom{N}{2}$ pairs, computing in $O(N^2)$ time. This is catastrophic for $N \ge 10^5$.
- instead, we decouple the summation into orthogonal bitwise dimensions.
- the XOR operator operates entirely independently across columns (bit positions).

- consider the $k$-th bit column across all $N$ numbers.
- let $C_1$ be the count of numbers where the $k$-th bit is `1`.
- let $C_0$ be the count of numbers where the $k$-th bit is `0`. Note that $C_0 = N - C_1$.

- for the $k$-th bit to contribute to a pair's XOR sum, the two numbers in the pair must differ at the $k$-th bit (one must be `1`, the other `0`).
- by basic combinatorics, the number of valid cross-pairs that yield a `1` in the $k$-th position is exactly $C_1 \times C_0$.
- each such pair contributes exactly $2^k$ to the total arithmetic sum.

- summing over all 32 orthogonal dimensions gives the final equation:
$$ \text{Total} = \sum_{k=0}^{31} (C_{1,k} \times C_{0,k}) \cdot 2^k $$


## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

long long sumXOR(vector<int>& arr) {
    long long total_sum = 0;
    int n = arr.size();
    
    // Evaluate orthogonally across all 32 bit planes
    for (int k = 0; k < 32; k++) {
        long long count1 = 0;
        
        for (int i = 0; i < n; i++) {
            if (arr[i] & (1 << k)) {
                count1++;
            }
        }
        
        long long count0 = n - count1;
        
        // Combinatorial cross pairs * positional weight
        total_sum += (count1 * count0) * (1LL << k);
    }
    
    return total_sum;
}
```


## Complexity Analysis
- **time Complexity:** $O(32 \cdot N) \approx O(N)$. The algorithm scans the array exactly 32 times, forming a linear projection constraint.
- **space Complexity:** $O(1)$ auxiliary space.

> [!warning]
> **Arithmetic Overflow:** The maximum sum scales asymptotically as $O(N^2 \cdot 2^{32})$. For $N = 10^5$, this exceeds $10^{19}$, which trivially overflows standard 32-bit `int` and presses the limits of 64-bit `long long`. Ensure `1LL << k` is used for shifting to prevent intermediate 32-bit positional truncation.

NEXT: [[Index]]
