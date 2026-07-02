---
type: concept
tags: [deque, sliding-window, monotonic-queue, cpp]
date: 2026-06-30
---
# Max Subarray Length with K Increments

## Problem Statement
Given an array $A$ of $N$ non-negative integers and an integer $K$, you are allowed to mathematically perform at most $K$ increment operations (where one operation increases a single element by $1$). Find the maximum length of a contiguous subarray such that all elements within the subarray can be made mathematically identical.

---

## Approach: Max Deque & Sliding Window Integral

To make all elements in a subarray $A[L \dots R]$ strictly identical using the minimum number of increments, we must elevate all elements to exactly $\max(A[L \dots R])$. 
The mathematical cost to homogenize the subarray is defined as:
$$ \text{Cost} = \sum_{i=L}^{R} (\max(A[L \dots R]) - A[i]) $$
$$ \text{Cost} = (\text{Length} \times \max(A[L \dots R])) - \sum_{i=L}^{R} A[i] $$

Our invariant requires that $\text{Cost} \le K$. To evaluate this constraint in strictly $O(1)$ time per window modification, we utilize a [[Prefix Sum]] to calculate the subarray sum instantly and a Monotonic [[Deque]] to track the absolute maximum of the current dynamic window.

1. **Monotonic Deque `max_dq`:** Maintains the indices of the dynamic window such that values are monotonically decreasing, keeping the window's maximum at `max_dq.front()`.
2. **Dynamic Sliding Window ($L$ and $R$):** 
   - Expand $R$, adding $A[R]$ to the `current_sum`.
   - Update `max_dq` by evicting elements smaller than $A[R]$.
   - Calculate mathematical homogeny cost: $(R - L + 1) \times A[\text{max\_dq.front()}] - \text{current\_sum}$.
   - If this cost exceeds $K$, the window is mathematically invalid. We must shrink it from the left.
   - During shrinking, subtract $A[L]$ from `current_sum`, and if `max_dq.front() == L`, pop it. Increment $L$.
3. **Record Global Maximum:** Update $\text{maxLength} = \max(\text{maxLength}, R - L + 1)$.

---

## Code Implementation

```cpp
#include <vector>
#include <deque>
#include <algorithm>
using namespace std;

int maxFrequency(const vector<int>& nums, long long k) {
    deque<int> max_dq;
    long long current_sum = 0;
    int L = 0;
    int maxLength = 0;
    int n = nums.size();
    
    for (int R = 0; R < n; ++R) {
        current_sum += nums[R];
        
        // Enforce monotonically decreasing invariant
        while (!max_dq.empty() && nums[max_dq.back()] <= nums[R]) {
            max_dq.pop_back();
        }
        max_dq.push_back(R);
        
        // Calculate Homogeny Cost
        long long max_val = nums[max_dq.front()];
        long long window_len = R - L + 1;
        long long cost = (window_len * max_val) - current_sum;
        
        // If cost strictly violates constraint K, shrink the topological window
        while (cost > k) {
            current_sum -= nums[L];
            
            if (max_dq.front() == L) {
                max_dq.pop_front();
            }
            
            L++;
            
            // Recalculate parameters for the modified window
            if (L <= R) {
                max_val = nums[max_dq.front()];
                window_len = R - L + 1;
                cost = (window_len * max_val) - current_sum;
            } else {
                cost = 0; // Collapsed window
            }
        }
        
        maxLength = max(maxLength, R - L + 1);
    }
    
    return maxLength;
}
```

> [!tip]
> A popular sub-variant of this problem involves sorting the array first (e.g., LeetCode 1838: Frequency of the Most Frequent Element). If the array is pre-sorted, the absolute maximum of the window is trivially $A[R]$, rendering the deque entirely unnecessary. The deque is only mathematically strictly required if the constraint explicitly demands preserving contiguous subarrays from an **unsorted** base sequence.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. Expanding $R$ operates strictly linearly. Shrinking $L$ processes each index at most once. Deque push/pop operations mathematically equate to amortized $O(1)$ per index.
- **Space Complexity:** $O(N)$. Memory occupied by the monotonic deque structure.
