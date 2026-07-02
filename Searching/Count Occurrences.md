---
type: concept
tags: [searching, cpp, binary-search, lower-bound, upper-bound]
date: 2026-07-01
---
# Count Occurrences

## Problem Statement
Given a sorted array $A$ of size $N$ and a target value $X$, mathematically determine the total number of times $X$ appears in the array.
*Example:* `[1, 1, 2, 2, 2, 2, 3]` and $X = 2$ returns $4$.

---

## Approach: Boundary Subtraction via Binary Search

Since the array is sorted, all occurrences of $X$ are perfectly contiguous. 
Instead of doing a linear scan $O(N)$ to count the elements, we can utilize $O(\log N)$ Binary Search to define the boundaries of the contiguous block.

The contiguous block starts at the **Lower Bound** (the first index $i$ where $A[i] \ge X$) and ends just before the **Upper Bound** (the first index $j$ where $A[j] > X$).

Mathematically, the number of occurrences is strictly the difference of the indices:
$$ \text{Count} = \text{Upper Bound Index} - \text{Lower Bound Index} $$

If the target does not exist in the array, both the Lower Bound and Upper Bound will return the exact same index, naturally resulting in a count of $0$.

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int countOccurrences(const vector<int>& nums, int target) {
    // Find the iterator pointing to the lower bound (first element >= target)
    auto lb = lower_bound(nums.begin(), nums.end(), target);
    
    // Find the iterator pointing to the upper bound (first element > target)
    auto ub = upper_bound(nums.begin(), nums.end(), target);
    
    // The total count is the mathematical difference between the two pointers
    return distance(lb, ub);
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(\log N)$. Both `std::lower_bound` and `std::upper_bound` execute binary searches taking $O(\log N)$ time. `std::distance` evaluates in $O(1)$ for random-access iterators (like vectors).
- **Space Complexity:** $O(1)$ auxiliary space.

> [!important]
> **Manual Implementation:** If an interviewer restricts the use of STL functions, you can write two separate manual binary search functions (`getLowerBound` and `getUpperBound`) as detailed in their respective notes, and simply return `getUpperBound(nums, target) - getLowerBound(nums, target)`.
