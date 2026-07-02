---
type: concept
tags: [two-pointer, cpp, sum-problem]
date: 2026-06-30
---
# Two Sum II - Input Array Is Sorted

## Problem Statement
Given a **1-indexed** array of integers $A$ that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target` number. Let these two numbers be $A[\text{index}_1]$ and $A[\text{index}_2]$ where $1 \le \text{index}_1 < \text{index}_2 \le |A|$. Return the indices $[\text{index}_1, \text{index}_2]$.

*Example:* $A = [2, 7, 11, 15]$, $\text{target} = 9$
*Result:* $[1, 2]$

---

## Approach: Pure Two Pointers

Because the array is already sorted, the monotonicity requirement of the [[Two-Pointer]] convergence technique is pre-satisfied. We do not need a hash map, and we do not need to sort the array ourselves. We can directly apply the two-pointer logic in strictly $O(N)$ time.

1. Initialize `left = 0` and `right = N - 1`.
2. Compute the mathematical state $S = A[\text{left}] + A[\text{right}]$.
3. If $S == \text{target}$, return the 1-based indices: `[left + 1, right + 1]`.
4. If $S < \text{target}$, the pair sum is deficient. We must increment `left` to introduce a larger addend into the pair.
5. If $S > \text{target}$, the pair sum is excessive. We must decrement `right` to introduce a smaller addend into the pair.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

vector<int> twoSumSorted(const vector<int>& numbers, int target) {
    int left = 0;
    int right = numbers.size() - 1;
    
    while (left < right) {
        // Prevent overflow if elements are near INT_MAX
        long long sum = (long long)numbers[left] + numbers[right];
        
        if (sum == target) {
            // Convert to 1-indexed format
            return {left + 1, right + 1};
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    
    // Mathematical guarantee states there is exactly one solution,
    // so we theoretically never reach here.
    return {};
}
```

> [!tip]
> This algorithm proves mathematically that if the array is sorted, searching for a pair sum takes precisely $O(N)$ comparisons without the $O(N)$ memory overhead required by a Hash Table.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. In the worst case, the pointers meet in the exact middle, visiting each element at most once.
- **Space Complexity:** $O(1)$. Only scalar index variables are maintained.
