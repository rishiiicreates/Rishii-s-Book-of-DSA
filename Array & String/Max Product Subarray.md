# Max Product Subarray

## Problem Statement
- given an integer array $A$ of size $N$, find a contiguous non-empty subarray within the array that has the largest product, and return that product.

- *example:* $A = [2, 3, -2, 4]$
- *result:* $6$ (Subarray $[2, 3]$)


## Approach: Dynamic Min and Max Tracking

- this is a variant of [[Kadane's Algorithm]], but with a critical twist: multiplying two negative numbers yields a positive number. A highly negative product can instantly become the absolute maximum if multiplied by another negative number.

- track `max_so_far`, `min_so_far`, and `result`, initialized to $A[0]$.
- iterate from $i=1$.
- when processing $A[i]$, we could extend the maximum product or the minimum product, or start fresh at $A[i]$.
- `temp_max = max(A[i], A[i] * max_so_far, A[i] * min_so_far)`
- `min_so_far = min(A[i], A[i] * max_so_far, A[i] * min_so_far)`
- `max_so_far = temp_max`
- update the global `result`.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

int maxProduct(const vector<int>& nums) {
    if (nums.empty()) return 0;
    
    int max_so_far = nums[0];
    int min_so_far = nums[0];
    int result = max_so_far;
    
    for (size_t i = 1; i < nums.size(); i++) {
        int curr = nums[i];
        
        // We must store the new max_so_far temporarily because we need the old max_so_far to calculate min_so_far
        int temp_max = max({curr, max_so_far * curr, min_so_far * curr});
        
        min_so_far = min({curr, max_so_far * curr, min_so_far * curr});
        max_so_far = temp_max;
        
        result = max(result, max_so_far);
    }
    
    return result;
}
```

> [!tip]
> `std::max({a, b, c})` and `std::min({a, b, c})` using initializer lists is an elegant C++11 feature to find the extremum among multiple values without nested function calls.

> [!important]
> Notice the use of `temp_max`. If we update `max_so_far` directly, the calculation of `min_so_far` on the very next line will use the mutated value, destroying the logic.


## Complexity Analysis
- **time Complexity:** $O(N)$ where $N$ is the number of elements. We pass through the array exactly once.
- **space Complexity:** $O(1)$. We only track three state variables.

NEXT: [[Index]]
