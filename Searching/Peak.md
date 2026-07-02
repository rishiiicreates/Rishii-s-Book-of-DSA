---
type: concept
tags: [searching, binary search, cpp]
date: 2026-06-30
---
# Find Peak Element

## Problem Statement
A peak element is an element that is strictly greater than its neighbors. Given a 0-indexed integer array $A$ where $A[i] \neq A[i+1]$, find a peak element and return its index. If the array contains multiple peaks, return the index to any of the peaks. You may imagine that $A[-1] = A[N] = -\infty$.

*Example:* $A = [1, 2, 1, 3, 5, 6, 4]$
*Result:* $1$ (value $2$) or $5$ (value $6$)

---

## Approach: Binary Search on Gradients (Optimal)

Although the array is not fully sorted, we can still use [[Binary Search]]. Because $A[-1]$ and $A[N]$ are $-\infty$, a peak is mathematically guaranteed to exist.

1. Find the `mid` element.
2. If $A[\text{mid}] < A[\text{mid} + 1]$, the slope is strictly increasing. Therefore, a peak *must* exist to the right of `mid`. We search the right half: `left = mid + 1`.
3. If $A[\text{mid}] > A[\text{mid} + 1]$, the slope is decreasing. A peak *must* exist to the left of `mid` (or `mid` itself is the peak). We search the left half: `right = mid`.
4. When `left == right`, we have pinpointed a peak.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

int findPeakElement(const vector<int>& nums) {
    int left = 0;
    int right = nums.size() - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        // Slope is increasing, peak must be strictly to the right
        if (nums[mid] < nums[mid + 1]) {
            left = mid + 1;
        } 
        // Slope is decreasing, peak is at mid or strictly to the left
        else {
            right = mid;
        }
    }
    
    return left; // or right, they are equal
}
```

> [!tip]
> This is a beautiful example of finding a property (a local maximum) in $O(\log N)$ time in an unsorted array based purely on the gradient of adjacent elements.

---

## Complexity Analysis
- **Time Complexity:** $O(\log_2 N)$. The search space is halved every iteration.
- **Space Complexity:** $O(1)$. Purely iterative variables are used.
