# Longest Subarray with Absolute Difference $\le X$

## Problem Statement
- given an array $A$ of $N$ integers and an integer $X$ (the limit), mathematically compute the length of the longest contiguous subarray such that the absolute difference between any two elements in this subarray is less than or equal to $X$. Formally, find the max length of subarray $A[L \dots R]$ where $\max(A[L \dots R]) - \min(A[L \dots R]) \le X$.


## Approach: Two Monotonic Deques (Sliding Window)

- to verify the mathematical constraint $\max(A[L \dots R]) - \min(A[L \dots R]) \le X$ in constant time as the window expands and contracts, we must dynamically track both the maximum and the minimum of the sliding window $A[L \dots R]$. This necessitates two separate [[Deque]] structures implementing dual Monotonic Queues.

- **dual Deque Setup:**
   - `max_dq`: Maintains a **monotonically decreasing** sequence to track the window's absolute maximum at its front.
   - `min_dq`: Maintains a **monotonically increasing** sequence to track the window's absolute minimum at its front.
- **expansion Phase ($R$ moves right):**
   - iterate a right pointer $R$ from $0$ to $N-1$.
   - maintain the monotonic invariants for both deques relative to $A[R]$.
   - push $R$ into both deques.
- **contraction Phase ($L$ moves right):**
   - after inserting $R$, check if the current window violates the constraint: $A[\text{max\_dq.front()}] - A[\text{min\_dq.front()}] > X$.
   - while the mathematical constraint is violated, the window must be shrunk.
   - if `max_dq.front() == L`, pop it from `max_dq` (since $L$ is leaving the window).
   - if `min_dq.front() == L`, pop it from `min_dq`.
   - increment $L$.
- **recording State:** After ensuring the window $[L \dots R]$ is mathematically valid, update the maximum length observed: $\text{maxLength} = \max(\text{maxLength}, R - L + 1)$.


## Code Implementation

```cpp
#include <vector>
#include <deque>
#include <algorithm>
using namespace std;

int longestSubarray(const vector<int>& nums, int limit) {
    deque<int> max_dq; // Stores indices, values monotonically decreasing
    deque<int> min_dq; // Stores indices, values monotonically increasing
    
    int L = 0;
    int maxLength = 0;
    int n = nums.size();
    
    for (int R = 0; R < n; ++R) {
        // Enforce decreasing invariant for max_dq
        while (!max_dq.empty() && nums[max_dq.back()] <= nums[R]) {
            max_dq.pop_back();
        }
        max_dq.push_back(R);
        
        // Enforce increasing invariant for min_dq
        while (!min_dq.empty() && nums[min_dq.back()] >= nums[R]) {
            min_dq.pop_back();
        }
        min_dq.push_back(R);
        
        // Mathematical Constraint Verification
        while (nums[max_dq.front()] - nums[min_dq.front()] > limit) {
            // Shrink the window from the left
            if (max_dq.front() == L) max_dq.pop_front();
            if (min_dq.front() == L) min_dq.pop_front();
            L++;
        }
        
        // Record maximal valid window length
        maxLength = max(maxLength, R - L + 1);
    }
    
    return maxLength;
}
```

> [!important]
> Unlike classical sliding window maximum problems where the window size $K$ is fixed, this algorithm utilizes a dynamic, fluid window. The elements are implicitly ejected not strictly by their age, but directly tied to the advancement of the scalar boundary $L$.


## Complexity Analysis
- **time Complexity:** $O(N)$. Both the $L$ and $R$ pointers iterate strictly forward. Every index is pushed and popped at most once in `max_dq` and `min_dq`. Thus, the amortized cost per element is $O(1)$.
- **space Complexity:** $O(N)$. In the worst-case, if the array is strictly monotonically increasing or decreasing, one of the deques could theoretically grow to encompass the entire array structure memory before ejection.

NEXT: [[Index]]
