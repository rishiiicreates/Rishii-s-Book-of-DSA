---
type: concept
tags: [sorting, cpp, arrays, two-pointers]
date: 2026-06-30
---
# Move Zeroes to End

## Problem Statement
Given an integer array `nums`, move all $0$s to the absolute end of the array while strictly preserving the relative ordered sequence of the non-zero elements.

---

## Approach: Stable Partitioning (Snowball Two-Pointer)

This operates similarly to the partitioning step in [[Quick Sort]], but it enforces a strict stability constraint.
We maintain an invariant boundary pointer `nonZeroBoundary` representing the index where the next incoming non-zero element should be deterministically placed.

Algorithm:
1. Iterate through the array using a fast scanner pointer `i`.
2. If `nums[i] != 0`, it is a valid element that must be shifted leftwards to preserve density.
3. Swap `nums[i]` with `nums[nonZeroBoundary]`.
4. Increment `nonZeroBoundary` to secure the element in the dense non-zero block.
5. All $0$s are mathematically bubbled to the right side of the boundary.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

void moveZeroes(vector<int>& nums) {
    // The invariant boundary tracking the dense non-zero block
    int nonZeroBoundary = 0;
    
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] != 0) {
            // Swap the non-zero element to the boundary
            swap(nums[nonZeroBoundary], nums[i]);
            nonZeroBoundary++;
        }
    }
}
```

> [!important]
> The `swap` ensures that if `i == nonZeroBoundary` (e.g., the array starts with non-zero elements), the element mathematically swaps with itself, maintaining stability and avoiding unnecessary structural shifts.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. The array is scanned deterministically in a single linear pass.
- **Space Complexity:** $\mathcal{O}(1)$. The algorithm requires strictly in-place boundary tracking.
