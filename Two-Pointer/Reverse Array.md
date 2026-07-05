# Reverse Array In-Place

## Problem Statement
- given an array of $N$ elements, mathematically invert its linear order strictly in-place, without allocating any supplementary structural memory.


## Approach: Symmetric Two-Pointer Swap

- an array can be logically inverted by symmetrically reflecting its elements across its central mathematical axis.
- we deploy a [[Two-Pointer]] convergence mechanism:
- initialize a left pointer $L = 0$ at the lower temporal boundary.
- initialize a right pointer $R = N - 1$ at the upper temporal boundary.
- symmetrically swap the elements residing at indices $L$ and $R$.
- increment $L$ and decrement $R$ strictly iteratively.
- the mathematical reflection terminates when $L \ge R$, representing convergence at the central axis.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

void reverseArray(vector<int>& arr) {
    int L = 0;
    int R = arr.size() - 1;
    
    // Symmetrical convergence boundary
    while (L < R) {
        // Swap values mathematically reflecting across the center
        swap(arr[L], arr[R]);
        L++;
        R--;
    }
}
```

> [!tip]
> For standard execution, the STL intrinsic `std::reverse(arr.begin(), arr.end())` utilizes exactly this optimized structural convergence internally. 


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. The two pointers mathematically converge exactly at $\lfloor N/2 \rfloor$, executing linearly bounded operations.
- **space Complexity:** $\mathcal{O}(1)$. Memory operations are strictly restricted to a single scalar temporary swap variable.

NEXT: [[Index]]
