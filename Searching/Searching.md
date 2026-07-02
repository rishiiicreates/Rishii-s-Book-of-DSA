---
type: concept
tags: [searching, cpp, binary-search]
date: 2026-06-30
---
# Searching (Binary Search)

## Problem Statement
Given a sorted array of integers $A$ of size $N$ and a target value $T$, write a function to search for $T$ in $A$. If $T$ exists, return its index. Otherwise, return $-1$.

*Example:* $A = [-1, 0, 3, 5, 9, 12]$, $T = 9$
*Result:* $4$

---

## Approach: Binary Search (Optimal)

Because the array is strictly sorted, we do not need to check every element $O(N)$. Instead, we can apply [[Binary Search]] to repeatedly halve the search space.

1. Initialize two pointers: `left = 0` and `right = N - 1`.
2. Find the middle index `mid`.
3. If $A[\text{mid}] == T$, we have found the target.
4. If $A[\text{mid}] < T$, the target must be in the right half. Update `left = mid + 1`.
5. If $A[\text{mid}] > T$, the target must be in the left half. Update `right = mid - 1`.
6. Terminate when `left > right`.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

int search(const vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size() - 1;
    
    while (left <= right) {
        // Prevents integer overflow that could happen with (left + right) / 2
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;
}
```

> [!important]
> Using `mid = left + (right - left) / 2` instead of `mid = (left + right) / 2` is extremely crucial in C++ to prevent [[Integer Overflow]] when `left` and `right` are both very large positive integers close to `INT_MAX`.

---

## Complexity Analysis
- **Time Complexity:** $O(\log_2 N)$. At each step, the search space is divided by 2.
- **Space Complexity:** $O(1)$. The iterative implementation uses no auxiliary space. (A recursive implementation would use $O(\log_2 N)$ stack space).
