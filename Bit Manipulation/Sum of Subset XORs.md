# Sum of Subset XORs

## Problem Statement
- given an array $A$ of $N$ integers, compute the mathematical sum of the XOR totals across all $2^N$ possible subsets.


## Approach: Probabilistic Bit Superposition

- a brute-force traversal of the $2^N$ power set takes $O(N \cdot 2^N)$ time. We can mathematically compress this to $O(N)$.
- we decouple the problem orthogonally by analyzing the contribution of the $k$-th bit.

- consider the $k$-th bit column across the array $A$.
- **case 1: The $k$-th bit is `0` in ALL elements.**
  - in this case, no subset can possibly have the $k$-th bit set. Its total combinatorial contribution is $0$.
- **case 2: The $k$-th bit is `1` in AT LEAST ONE element.**
  - if there is at least one element with the $k$-th bit set, then exactly half of the $2^N$ subsets will have an odd number of `1`s at the $k$-th position (thus XORing to `1`), and the other half will have an even number (XORing to `0`).
  - therefore, exactly $2^{N-1}$ subsets will have the $k$-th bit set.
  - the positional contribution of the $k$-th bit is $2^k \times 2^{N-1}$.

- summing this logic over all 32 bits:
- the bitwise OR operator $A_0 \lor A_1 \dots \lor A_{N-1}$ creates a mask of all bits that fall into Case 2.
- thus, the final sum is simply:
$$ \text{Total} = (A_0 \lor A_1 \dots \lor A_{N-1}) \times 2^{N-1} $$


## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

long long subsetXORSum(vector<int>& nums) {
    int or_sum = 0;
    int n = nums.size();
    
    // Collapse to global superposition state
    for (int num : nums) {
        or_sum |= num;
    }
    
    // Multiply by 2^(N-1). Use long long to avoid shift overflow
    return (long long)or_sum * (1LL << (n - 1));
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ strict linear scan to collapse the OR superposition.
- **space Complexity:** $O(1)$ auxiliary space.

> [!important]
> **Exponential Shift Overflow:** The multiplier $2^{N-1}$ grows exponentially. For $N > 62$, `1LL << (N - 1)` triggers undefined behavior on 64-bit architectures, and the total sum exceeds 64-bit numerical limits. Modular arithmetic is fundamentally required if bounds scale beyond $N=60$.

NEXT: [[Index]]
