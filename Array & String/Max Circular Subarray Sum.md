---
type: concept
tags: [array, string, cpp, subarray]
date: 2026-06-30
---
# Max Circular Subarray Sum

## Problem Statement
Given a circular integer array $A$ of size $N$, find the maximum possible sum of a non-empty contiguous subarray. A circular array means the end of the array connects to the beginning.

*Example:* $A = [5, -3, 5]$
*Result:* $10$ (Subarray $[5, 5]$ wrapping around)

---

## Approach: Max / Min Kadane's

The maximum subarray sum in a circular array can take two forms:
1. **Linear Form:** The subarray does not wrap around. The answer is just the standard result from [[Kadane's Algorithm]].
2. **Circular Form:** The subarray wraps around the edges. This means the elements *excluded* from our sum form a standard, non-wrapping contiguous subarray in the middle. To maximize the wrapping sum, we must minimize the excluded middle sum.

Therefore, `Circular Max = Total Array Sum - Minimum Subarray Sum`.
We run Kadane's Algorithm for both maximum and minimum simultaneously.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

int maxSubarraySumCircular(const vector<int>& nums) {
    int total_sum = 0;
    int curr_max = 0, max_sum = nums[0];
    int curr_min = 0, min_sum = nums[0];
    
    for (int x : nums) {
        // Standard Kadane's for Maximum
        curr_max = max(x, curr_max + x);
        max_sum = max(max_sum, curr_max);
        
        // Kadane's for Minimum
        curr_min = min(x, curr_min + x);
        min_sum = min(min_sum, curr_min);
        
        total_sum += x;
    }
    
    // Edge case: All numbers are negative
    if (max_sum < 0) {
        return max_sum;
    }
    
    return max(max_sum, total_sum - min_sum);
}
```

> [!warning]
> The edge case where **all** elements are negative is critical. In this scenario, `total_sum == min_sum`. Thus, `total_sum - min_sum = 0`. But a subarray cannot be empty! So if `max_sum < 0`, we must return `max_sum` directly instead of returning $0$.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ where $N$ is the number of elements. We compute everything in a single pass.
- **Space Complexity:** $O(1)$. No auxiliary structures are used.
