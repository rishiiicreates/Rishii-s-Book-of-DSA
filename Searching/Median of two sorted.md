---
type: concept
tags: [searching, cpp, binary-search, divide-and-conquer, array]
date: 2026-07-01
---
# Median of Two Sorted Arrays

## Problem Statement
Given two sorted arrays $A$ and $B$ of sizes $N$ and $M$ respectively, find the exact mathematical median of the two sorted arrays when merged.
The time complexity must strictly be bounded by $O(\log(\min(N, M)))$.

---

## Approach: Partitioning via Binary Search

The median divides a sorted set exactly in half. For two arrays, we can conceptualize this as finding a partition cut across both arrays.
Let the required number of elements in the left half be $K = \lfloor \frac{N + M + 1}{2} \rfloor$. This handles both even and odd total lengths gracefully.

Assume we make a cut in $A$ at index $i$. The cut in $B$ must mathematically be at index $j = K - i$.
This partition is valid if and only if:
1. $A[i-1] \le B[j]$ (The largest left element in $A$ is $\le$ the smallest right element in $B$)
2. $B[j-1] \le A[i]$ (The largest left element in $B$ is $\le$ the smallest right element in $A$)

Since $A$ and $B$ are sorted, we can binary search the optimal cut $i$ in the smaller array.
1. Force $A$ to be the smaller array to guarantee $O(\log(\min(N, M)))$ bound and ensure $j \ge 0$.
2. Initialize search bounds for the cut in $A$: $L = 0$ and $R = N$.
3. For $i = L + \lfloor (R - L) / 2 \rfloor$ and $j = K - i$:
   - Extract bounds $l_1 = A[i-1]$, $l_2 = B[j-1]$, $r_1 = A[i]$, $r_2 = B[j]$. (Substitute $\pm\infty$ for out-of-bounds indices).
   - If $l_1 \le r_2$ and $l_2 \le r_1$: Valid partition found.
     - If $N+M$ is odd, median is $\max(l_1, l_2)$.
     - If $N+M$ is even, median is $\frac{\max(l_1, l_2) + \min(r_1, r_2)}{2.0}$.
   - If $l_1 > r_2$: The cut in $A$ is too far right. Move left ($R = i - 1$).
   - If $l_2 > r_1$: The cut in $A$ is too far left. Move right ($L = i + 1$).

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>

using namespace std;

double findMedianSortedArrays(const vector<int>& nums1, const vector<int>& nums2) {
    int n = nums1.size();
    int m = nums2.size();
    
    // Always binary search on the smaller array
    if (n > m) return findMedianSortedArrays(nums2, nums1);
    
    int low = 0, high = n;
    int k = (n + m + 1) / 2;
    
    while (low <= high) {
        int cut1 = low + (high - low) / 2;
        int cut2 = k - cut1;
        
        int l1 = (cut1 == 0) ? INT_MIN : nums1[cut1 - 1];
        int l2 = (cut2 == 0) ? INT_MIN : nums2[cut2 - 1];
        int r1 = (cut1 == n) ? INT_MAX : nums1[cut1];
        int r2 = (cut2 == m) ? INT_MAX : nums2[cut2];
        
        if (l1 <= r2 && l2 <= r1) {
            if ((n + m) % 2 == 1) {
                return max(l1, l2);
            } else {
                return (max(l1, l2) + min(r1, r2)) / 2.0;
            }
        }
        else if (l1 > r2) {
            high = cut1 - 1;
        }
        else {
            low = cut1 + 1;
        }
    }
    
    return 0.0;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(\log(\min(N, M)))$ as we execute a standard binary search on the smallest array.
- **Space Complexity:** $O(1)$ auxiliary space.

> [!important]
> **Infinity Bounds Substitution:** The variables $l_1, l_2, r_1, r_2$ conceptually represent the bounds. If a cut is at $0$, there are no elements on the left, so $l = -\infty$. If a cut is at the end, there are no elements on the right, so $r = +\infty$. This mathematical substitution allows the core comparison logic to remain clean without nested out-of-bounds `if` statements.
