# Max sum of i*arr[i]

## Problem Statement
- given an array $A$ of $N$ integers, compute the maximum possible value of $\sum_{i=0}^{N-1} (i \times A[i])$ achievable by rotating the array any number of times.


## Approach: Mathematical Recurrence Relation

- a naive approach involves rotating the array $N$ times and computing the sum for each configuration, taking $O(N^2)$ time.
- we can optimize this to $O(N)$ by deriving a mathematical recurrence relation between consecutive rotations.

- let $S_0$ be the sum for the initial array:
$$ S_0 = 0 \times A[0] + 1 \times A[1] + 2 \times A[2] + \dots + (N-1) \times A[N-1] $$

- if we rotate the array to the right by one position, the new sum $S_1$ becomes:
$$ S_1 = 0 \times A[N-1] + 1 \times A[0] + 2 \times A[1] + \dots + (N-1) \times A[N-2] $$

- let's find the mathematical difference $S_1 - S_0$:
$$ S_1 - S_0 = A[0] + A[1] + \dots + A[N-2] - (N-1) \times A[N-1] $$

- notice that $A[0] + A[1] + \dots + A[N-2]$ is exactly the total sum of the array minus $A[N-1]$.
- let $T = \sum_{i=0}^{N-1} A[i]$.
- then $S_1 - S_0 = T - A[N-1] - (N-1) \times A[N-1] = T - N \times A[N-1]$.

- generalizing for the $k$-th rotation:
$$ S_k = S_{k-1} + T - N \times A[N-k] $$

- this $O(1)$ state transition allows us to compute all rotations in $O(N)$ time.


## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

long long maxSum(const vector<int>& arr) {
    int n = arr.size();
    if (n == 0) return 0;
    
    long long total_sum = 0;
    long long current_val = 0;
    
    // Compute T (total_sum) and S_0 (current_val)
    for (int i = 0; i < n; ++i) {
        total_sum += arr[i];
        current_val += 1LL * i * arr[i]; // 1LL prevents integer overflow
    }
    
    long long max_val = current_val;
    
    // Apply the recurrence relation for the remaining N-1 rotations
    for (int i = 1; i < n; ++i) {
        current_val = current_val + total_sum - 1LL * n * arr[n - i];
        max_val = max(max_val, current_val);
    }
    
    return max_val;
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ as we calculate the initial values in one pass and then evaluate all state transitions in a second pass.
- **space Complexity:** $O(1)$ auxiliary space.

> [!important]
> **Integer Overflow:** Because we multiply array values by their index $i$ (up to $10^5$), the sum can easily exceed the 32-bit signed integer limit ($2 \times 10^9$). You must use 64-bit integers (`long long` in C++) for `total_sum`, `current_val`, and intermediate arithmetic.

NEXT: [[Index]]
