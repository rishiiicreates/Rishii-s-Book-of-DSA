# Sum of All Subarrays

## Problem Statement
- given an array $A$ of size $N$, calculate the total sum of all possible contiguous subarrays.

- *example:* $A = [1, 2, 3]$
- *result:* $20$ (Subarrays are $[1], [2], [3], [1,2], [2,3], [1,2,3]$. Sum = $1+2+3+3+5+6 = 20$)


## Approach: Mathematical Contribution (Optimal)

- a naive approach generating all subarrays would take $O(N^2)$ or $O(N^3)$ time. However, using a pure mathematical approach, we can calculate how many times a particular element $A[i]$ appears across all possible subarrays.

- for an element at index $i$ (0-indexed):
- it can be part of any subarray that starts at or before $i$. There are $(i + 1)$ such starting positions.
- it can be part of any subarray that ends at or after $i$. There are $(N - i)$ such ending positions.
- total occurrences of $A[i]$ in all subarrays = $(i + 1) \times (N - i)$.
- its contribution to the total sum is $A[i] \times (i + 1) \times (N - i)$.


## Code Implementation

```cpp
#include <vector>
using namespace std;

long long subarraySum(const vector<int>& arr) {
    long long total_sum = 0;
    long long n = arr.size();
    
    for (long long i = 0; i < n; i++) {
        // Calculate the number of times arr[i] appears in all subarrays
        long long occurrences = (i + 1) * (n - i);
        
        // Add its total contribution to the answer
        total_sum += arr[i] * occurrences;
    }
    
    return total_sum;
}
```

> [!warning]
> **Integer Overflow:** The number of occurrences $(i+1) \times (N-i)$ can easily exceed the capacity of a 32-bit signed integer. It is strictly required to cast $N$ and $i$ to `long long` (or `size_t`) before multiplication.


## Complexity Analysis
- **time Complexity:** $O(N)$. We make a single pass through the array.
- **space Complexity:** $O(1)$. We only compute running totals algebraically.

NEXT: [[Index]]
