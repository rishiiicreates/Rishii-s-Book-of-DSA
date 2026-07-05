# Find Position of Only Set Bit

## Problem Statement
- given an integer $N$, determine the 1-indexed position of its only set bit. If $N$ has zero set bits, or more than one set bit, mathematically reject the input by returning $-1$.


## Approach: Power of Two Assertion & Logarithmic Extraction

- a number $N$ possesses exactly one set bit if and only if it is a strict positive integer power of $2$ ($N = 2^k$ for some $k \ge 0$).
- before attempting to find the bit, we must rigorously assert that $N$ is valid.

- **assertion Constraint:**
- we use Brian Kernighan's bit trick: $N \land (N-1)$ strips the lowest set bit from $N$.
- if $N$ has exactly one set bit, $N \land (N-1)$ must yield $0$.
- constraint: `N > 0 && (N & (N - 1)) == 0`.

- **position Extraction:**
- if $N = 2^k$, we can isolate $k$ by taking the base-2 logarithm:
$$ k = \log_2(N) $$
- since the problem asks for the 1-indexed position, the final mathematical position is exactly $k + 1 = \log_2(N) + 1$.


## Code Implementation

```cpp
#include <iostream>
#include <cmath>

using namespace std;

int findPosition(int n) {
    // Assert N is strictly a power of two
    if (n <= 0 || (n & (n - 1)) != 0) {
        return -1;
    }
    
    // Extract the exponent k = log2(N)
    // 1-indexed position is k + 1
    return log2(n) + 1;
}
```


## Complexity Analysis
- **time Complexity:** $O(1)$. Floating-point $\log_2$ is evaluated in constant time by the FPU. Alternatively, `__builtin_ctz(n)` runs in a single CPU instruction.
- **space Complexity:** $O(1)$ auxiliary space.

> [!tip]
> **Hardware Intrinsics:** While `std::log2` works perfectly, hardware intrinsics are mathematically pure and faster. GCC and Clang provide `__builtin_ctz(n)` (Count Trailing Zeros). If $N = 2^k$, the number of trailing zeroes is exactly $k$. Thus, the 1-indexed position is simply `__builtin_ctz(n) + 1`.

NEXT: [[Index]]
