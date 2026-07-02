---
type: concept
tags: [two-pointer, sliding-window, cpp, math, array]
date: 2026-06-30
---
# Max Consecutive Ones II

## Problem Statement
Given a binary array consisting strictly of $0$s and $1$s, determine the mathematical maximum length of a contiguous subarray containing only $1$s, given that you are permitted to structurally flip at most exactly one $0$ into a $1$.

---

## Approach: Dynamic Sliding Window (Two Pointers)

Instead of physically mutating the array, we reformulate the problem mathematically: Find the longest contiguous geometric window $[L, R]$ such that the absolute count of $0$s within the bounds is strictly $\le 1$.

Algorithm:
1. Initialize a spatial window defined by pointers $L = 0$ and $R = 0$.
2. Maintain a scalar state variable `zero_count` to mathematically track the structural violations within the window.
3. Iteratively expand the window by incrementing $R$. If `arr[R] == 0`, increment `zero_count`.
4. **Boundary Violation Constraint:** If `zero_count > 1`, the mathematical invariant is broken. We must forcefully shrink the window from the left by incrementing $L$. If `arr[L] == 0` during this contraction, decrement `zero_count`.
5. The maximum length dynamically bounded by $R - L + 1$ is globally tracked whenever the invariant strictly holds (`zero_count <= 1`).

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

int findMaxConsecutiveOnes(const vector<int>& nums) {
    int max_len = 0;
    int left = 0;
    int zero_count = 0;
    int n = nums.size();
    
    for (int right = 0; right < n; ++right) {
        // Accumulate state violations
        if (nums[right] == 0) {
            zero_count++;
        }
        
        // Restore mathematical constraint via window contraction
        while (zero_count > 1) {
            if (nums[left] == 0) {
                zero_count--;
            }
            left++; // Dynamically shrink left bound
        }
        
        // Evaluate maximal spatial geometry
        max_len = max(max_len, right - left + 1);
    }
    
    return max_len;
}
```

> [!tip]
> A highly optimized variant replaces the inner `while` loop with a singular `if (zero_count > 1)` check. Because we only care about the absolute theoretical maximum length, if the window becomes invalid, we can shift the *entire* bounded window (both $L$ and $R$ incrementing simultaneously) without explicitly shrinking it. This mathematically preserves the maximum theoretical spatial state found thus far.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. Both the $L$ and $R$ pointers traverse the sequence monotonically. The inner `while` loop ensures each element is inspected strictly at most twice.
- **Space Complexity:** $\mathcal{O}(1)$. Evaluated strictly in bounded scalar registers.
