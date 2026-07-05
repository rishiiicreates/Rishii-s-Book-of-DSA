# Wave Form

## Problem Statement
- given an array $A$ of $N$ distinct integers, reorder it in-place into a mathematical **Wave Form**.
- a wave form is defined by the oscillating inequality:
$$ A[0] \ge A[1] \le A[2] \ge A[3] \le A[4] \dots $$
- in general, for all even indices $i$, $A[i]$ must be a local maximum: $A[i] \ge A[i-1]$ and $A[i] \ge A[i+1]$.


## Approach: Local Optimization vs Global Sort

- a naive approach takes $O(N \log N)$ time: sort the array globally, and then swap adjacent pairs (swap $A[0]$ with $A[1]$, $A[2]$ with $A[3]$, etc.).
- however, global sorting is mathematically overkill because the problem only dictates **local** constraints.

- we can achieve this in strict $O(N)$ time by iterating only on the even indices $i \in \{0, 2, 4, \dots \}$ and aggressively enforcing the local maximum property:
- if $A[i]$ is smaller than its left neighbor $A[i-1]$, swap them.
- if $A[i]$ is smaller than its right neighbor $A[i+1]$, swap them.

- why does this local swapping not break previous constraints?
- assume at index $i$, we swap $A[i]$ with $A[i-1]$. This only makes $A[i]$ larger and $A[i-1]$ smaller. Since $A[i-1]$ was already required to be a local minimum, making it even smaller strictly reinforces the prior inequality $A[i-2] \ge A[i-1]$.


## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

void sortInWave(vector<int>& arr) {
    int n = arr.size();
    
    // Iterate only over even indices, forcing them to be local peaks
    for (int i = 0; i < n; i += 2) {
        
        // Ensure greater than left neighbor
        if (i > 0 && arr[i] < arr[i - 1]) {
            swap(arr[i], arr[i - 1]);
        }
        
        // Ensure greater than right neighbor
        if (i < n - 1 && arr[i] < arr[i + 1]) {
            swap(arr[i], arr[i + 1]);
        }
    }
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$. We perform a single linear scan, jumping by $2$.
- **space Complexity:** $O(1)$ auxiliary space.

> [!tip]
> **Constraint Relaxation:** By recognizing that the wave form property is defined purely by adjacent trios $(A[i-1], A[i], A[i+1])$, we reduced a seemingly global sorting problem into a linear $O(N)$ state machine resolution. Always check if a sorting constraint is truly global before calling `std::sort`.

NEXT: [[Index]]
