# Missing Number

## Problem Statement
- given an array containing $N$ distinct numbers mathematically bounded within the domain $[0, N]$, identify the exact missing integer.


## Approach: XOR Identity Collapse

- while one could compute the arithmetic sum of the first $N$ integers using Gauss's formula $\frac{N(N+1)}{2}$ and subtract the array sum, this risks integer overflow for extremely large $N$.

- a structurally pure mathematical approach utilizes the XOR operator ($\oplus$).
- recall the Abelian group axioms of XOR:
- $x \oplus X = 0$ (Nilpotence)
- $x \oplus 0 = X$ (Identity)
- $(x \oplus Y) \oplus Z = X \oplus (Y \oplus Z)$ (Associativity)

- let $S$ be the full sequence $[0, N]$. Let $A$ be the given array sequence.
- we construct a global XOR accumulator:
$$ \text{Result} = \left( \bigoplus_{i=0}^N S_i \right) \oplus \left( \bigoplus_{j=0}^{N-1} A_j \right) $$

- because XOR is commutative and associative, every element present in $A$ will pair with its identical counterpart in $S$.
- by nilpotence, they annihilate to $0$.
- the single missing element $M$ has no pair, leaving $0 \oplus M = M$.


## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

int missingNumber(vector<int>& nums) {
    int missing = nums.size(); // Initialize with N
    
    for (int i = 0; i < nums.size(); i++) {
        // XOR the index i (from sequence S) and the value (from array A)
        missing ^= i ^ nums[i];
    }
    
    return missing;
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ linear scan to compute the cumulative XOR.
- **space Complexity:** $O(1)$ auxiliary memory.

> [!tip]
> **Overflow Immunity:** The mathematical summation approach requires up to $\sim N^2$ bit capacity, which risks overflow if not strictly typed as a 64-bit integer. The XOR approach operates strictly within the bit-width of $N$, rendering it mathematically immune to overflow regardless of scale.

NEXT: [[Index]]
