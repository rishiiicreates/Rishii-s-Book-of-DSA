---
type: concept
tags: [searching, cpp, binary-search]
date: 2026-06-30
---
# Insertion Position

## Problem Statement
Given a sorted array of distinct integers $A$ and a target value $T$, return the index if the target is found. If not, return the index where it would be if it were inserted in order. You must write an algorithm with $O(\log N)$ runtime complexity.

*Example:* $A = [1, 3, 5, 6]$, $T = 5 \rightarrow$ Result: $2$
*Example:* $A = [1, 3, 5, 6]$, $T = 2 \rightarrow$ Result: $1$

---

## Approach: Lower Bound (Binary Search)

This problem asks us to find the smallest index $i$ such that $A[i] \ge T$. This is formally known as finding the **Lower Bound**. We can achieve this via a modified [[Binary Search]].

1. Set `left = 0` and `right = N`. (Notice `right` is $N$, not $N-1$, because if the element is strictly greater than all elements, it gets inserted at the very end).
2. Calculate `mid`.
3. If $A[\text{mid}] < T$, then all indices up to `mid` are invalid. The insertion point must be to the right: `left = mid + 1`.
4. If $A[\text{mid}] \ge T$, then `mid` is a valid candidate for the insertion point. We discard the right half to see if there's a smaller valid index: `right = mid`.
5. Terminate when `left == right`.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

int searchInsert(const vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size(); // Can be inserted at the very end
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return left;
}
```

> [!tip]
> In C++ STL, this exact functionality is implemented internally in `std::lower_bound(nums.begin(), nums.end(), target)`. To get the raw index, you simply subtract the beginning iterator:
> `return lower_bound(nums.begin(), nums.end(), target) - nums.begin();`

---

## Complexity Analysis
- **Time Complexity:** $O(\log_2 N)$. The binary search cuts the interval in half every iteration.
- **Space Complexity:** $O(1)$. Auxiliary space is strictly constant.
