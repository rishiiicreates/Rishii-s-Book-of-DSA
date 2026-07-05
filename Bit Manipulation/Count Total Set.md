# Count Total Set Bits

## Problem Statement
- given a mathematical integer $N$, compute the total sum of set bits (`1`s) across all integers in the contiguous sequence $[1, N]$.


## Approach: Logarithmic Divide and Conquer

- a naive iteration takes $O(N \log N)$ time, which fails for large $N$.
- we use a geometric approach by observing the recursive fractal nature of binary representations.

- for a given $N$, let $x = \lfloor \log_2 N \rfloor$. This means $2^x$ is the largest power of two completely contained within $N$.
- we partition the sequence $[1, N]$ into three mathematical domains:
- **domain 1: $[1, 2^x - 1]$**.
   - this is a complete binary tree of depth $x$. The number of elements is $2^x - 1$. Each column of bits has exactly half $1$s and half $0$s.
   - total bits = $x \cdot 2^{x-1}$.
- **domain 2: $[2^x, N]$ MSBs**.
   - every number in this range has its $x$-th bit (Most Significant Bit) strictly set to $1$.
   - the count of these MSBs is $(N - 2^x) + 1$.
- **domain 3: The Remainder**.
   - if we strip the MSB from the numbers in $[2^x, N]$, the remaining bit patterns exactly mirror the sequence $[1, N - 2^x]$.
   - we recursively solve for $N - 2^x$.

- recurrence Relation:
$$ f(N) = x \cdot 2^{x-1} + (N - 2^x + 1) + f(N - 2^x) $$


## Code Implementation

```cpp
#include <iostream>

using namespace std;

// Extracts x = floor(log2(n))
int getHighestPowerOf2(int n) {
    int x = 0;
    while ((1 << x) <= n) {
        x++;
    }
    return x - 1;
}

int countTotalSetBits(int n) {
    if (n == 0) return 0;
    
    int x = getHighestPowerOf2(n);
    
    int bitsUpTo2x = x * (1 << (x - 1));
    int msbFrom2xToN = n - (1 << x) + 1;
    int remaining = n - (1 << x);
    
    return bitsUpTo2x + msbFrom2xToN + countTotalSetBits(remaining);
}
```


## Complexity Analysis
- **time Complexity:** $O(\log N)$. At each step, we strip the highest bit of $N$. The recursion depth is bounded by the number of bits in $N$.
- **space Complexity:** $O(\log N)$ auxiliary space due to the Call Stack. This can be flattened to $O(1)$ iteratively.

NEXT: [[Index]]
