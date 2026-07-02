---
type: concept
tags: [array, cpp, kadane, dp, subarray]
date: 2026-06-30
---
# Kadane's Algorithm

## Problem Statement
Given an integer array $A$ of size $N$, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

*Example:* $A = [-2, 1, -3, 4, -1, 2, 1, -5, 4]$
*Result:* $6$ (The subarray is $[4, -1, 2, 1]$)

---

## Approach: Kadane's Algorithm (Optimal)

[[Kadane's Algorithm]] is a dynamic programming approach that operates on the fact that any negative subarray sum will never contribute positively to a future contiguous subarray. 

1. Maintain a `current_sum` and a `max_sum`, both initialized to $A[0]$.
2. Iterate through the array starting from $i=1$.
3. At each step, we have a choice:
   - Add $A[i]$ to the existing subarray (`current_sum + A[i]`)
   - Start a new subarray strictly at $A[i]$
4. `current_sum = max(A[i], current_sum + A[i])`
5. `max_sum = max(max_sum, current_sum)`

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

int maxSubArray(const vector<int>& nums) {
    // Initialize both with the first element
    int current_sum = nums[0];
    int max_sum = nums[0];
    
    for (size_t i = 1; i < nums.size(); i++) {
        // Decide whether to extend the previous subarray or start a new one
        current_sum = max(nums[i], current_sum + nums[i]);
        
        // Update the global maximum
        max_sum = max(max_sum, current_sum);
    }
    
    return max_sum;
}
```

> [!important]
> What if all numbers in the array are negative? Kadane's algorithm works flawlessly because `current_sum` will simply pick the least negative number (resetting itself instead of accumulating a massive negative debt), and `max_sum` will capture it.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ where $N$ is the size of the array. The algorithm processes every element exactly once.
- **Space Complexity:** $O(1)$. No auxiliary data structures are used.
