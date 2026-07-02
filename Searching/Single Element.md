---
type: concept
tags: [searching, cpp, binary-search, math]
date: 2026-07-01
---
# Single Element

## Problem Statement
Given a sorted array $A$ where every element appears exactly twice, except for one element which appears exactly once, find the single element.
*Example:* `[1, 1, 2, 3, 3, 4, 4, 8, 8]` returns `2`.

---

## Approach: Index Parity using Binary Search

A naive approach using bitwise XOR takes $O(N)$ time. Since the array is sorted, we can optimize this to $O(\log N)$ using **Binary Search** by analyzing the parity of indices.

Because all numbers appear in pairs, the pairs naturally align on `(even, odd)` index boundaries.
For any pair $(x, x)$:
- **Before the single element:** The first instance of a pair is at an **even** index, and the second is at an **odd** index. (e.g., $A[0]=A[1]$, $A[2]=A[3]$).
- **After the single element:** The presence of the single element shifts everything by 1. Therefore, the first instance of a pair is at an **odd** index, and the second is at an **even** index. (e.g., $A[3]=A[4]$, $A[5]=A[6]$).

We can binary search the midpoint $M$:
1. If $M$ is odd, its paired counterpart *should* be $M-1$.
2. If $M$ is even, its paired counterpart *should* be $M+1$.
We can consolidate this using bitwise XOR: The expected partner of index $M$ is $M \oplus 1$.

If $A[M] == A[M \oplus 1]$, we are in the **left half** (before the single element). We move right: `low = mid + 1`.
If $A[M] \ne A[M \oplus 1]$, we are in the **right half** (after or at the single element). We move left: `high = mid - 1`.

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

int singleNonDuplicate(const vector<int>& nums) {
    int n = nums.size();
    
    // Base cases to avoid out-of-bounds checks inside the loop
    if (n == 1) return nums[0];
    if (nums[0] != nums[1]) return nums[0];
    if (nums[n - 1] != nums[n - 2]) return nums[n - 1];
    
    int low = 1, high = n - 2;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        // If mid element doesn't match its left or right, it is the single element
        if (nums[mid] != nums[mid - 1] && nums[mid] != nums[mid + 1]) {
            return nums[mid];
        }
        
        // (mid ^ 1) gives mid-1 if mid is odd, and mid+1 if mid is even
        if (nums[mid] == nums[mid ^ 1]) {
            // We are in the left half, single element is to the right
            low = mid + 1;
        } else {
            // We are in the right half, single element is to the left
            high = mid - 1;
        }
    }
    
    return -1; // Should theoretically never be reached
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(\log N)$ due to standard binary search halving.
- **Space Complexity:** $O(1)$ auxiliary space.

> [!tip]
> **XOR Parity Trick:** The operation `mid ^ 1` is an incredibly elegant mathematical trick in C++. If `mid` is even, `mid ^ 1` adds 1. If `mid` is odd, `mid ^ 1` subtracts 1. This elegantly toggles between the expected bounds of a pair without messy `if-else` blocks.
