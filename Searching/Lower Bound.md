---
type: concept
tags: [searching, cpp, binary-search, lower-bound]
date: 2026-07-01
---
# Lower Bound

## Problem Statement
Given a sorted array $A$ of size $N$ and a target value $X$, mathematically define and find the **Lower Bound** of $X$ in $A$.
The lower bound is defined as the smallest index $i \in [0, N-1]$ such that $A[i] \ge X$.
If no such element exists, the lower bound is conceptually at index $N$.

---

## Approach: Binary Search

A naive linear search would take $O(N)$ time. Since the array is monotonically non-decreasing, we can apply **Binary Search** to find the threshold index in $O(\log N)$ time.

1. Initialize two pointers: $L = 0$ and $R = N - 1$. Maintain a potential `ans` variable initialized to $N$ (the default if all elements are strictly less than $X$).
2. While $L \le R$:
   - Calculate the midpoint $M = L + \lfloor (R - L) / 2 \rfloor$.
   - If $A[M] \ge X$: We found a potential valid index. We record `ans = M` and search the left half by setting $R = M - 1$ to find an even smaller index.
   - If $A[M] < X$: The element is strictly smaller, so the threshold must be in the right half. Set $L = M + 1$.

*Note:* In modern C++, the STL algorithm `std::lower_bound` implements this exact logic.

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Manual Binary Search Implementation
int getLowerBoundManual(const vector<int>& nums, int target) {
    int n = nums.size();
    int low = 0, high = n - 1;
    int ans = n;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (nums[mid] >= target) {
            ans = mid;         // Record potential answer
            high = mid - 1;    // Continue searching in the left half
        } else {
            low = mid + 1;     // Target is greater, search right half
        }
    }
    
    return ans;
}

// STL Implementation
int getLowerBoundSTL(const vector<int>& nums, int target) {
    auto it = lower_bound(nums.begin(), nums.end(), target);
    return distance(nums.begin(), it);
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(\log N)$ because the search space is halved at each step.
- **Space Complexity:** $O(1)$ auxiliary space.

> [!important]
> **Out of Bounds Target:** If $X$ is strictly greater than all elements in $A$, `std::lower_bound` returns `nums.end()`. In our manual implementation, it returns $N$. Both represent the mathematical limit beyond the array boundary. Always check `if (ans < n)` before accessing `nums[ans]`.
