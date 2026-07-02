---
type: concept
tags: [searching, binary-search, cpp]
date: 2026-06-30
---
# Search in Almost Sorted Array

## Problem Statement
Given an array which is initially sorted but has been slightly perturbed such that every element can be at most one position away from its correctly sorted position (i.e., `arr[i]` could only be swapped with `arr[i-1]` or `arr[i+1]`), find the index of a `target` element.

---

## Approach: Tri-Point Bounding Search

Since the array is only "almost" sorted, a standard binary search might miss the target if it was swapped across the `mid` boundary. 
The mathematical property of this perturbation is strictly localized: any element's true sorted position is bounded within a radius of 1.

Therefore, for any `mid` evaluated, we must aggressively check the triad of indices: `[mid - 1, mid, mid + 1]`.
1. Check `arr[mid]`.
2. Check `arr[mid - 1]` (ensuring `mid > left`).
3. Check `arr[mid + 1]` (ensuring `mid < right`).

If the target is not found in the triad, we can safely prune the search space. To avoid reprocessing the already checked boundary elements, we aggressively jump the pointers by 2:
- If `target < arr[mid]`, the target mathematically must reside in the left half, so `right = mid - 2`.
- If `target > arr[mid]`, the target resides in the right half, so `left = mid + 2`.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

int searchAlmostSorted(const vector<int>& arr, int target) {
    if (arr.empty()) return -1;
    
    int left = 0;
    int right = arr.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        // Tri-Point Validation
        if (arr[mid] == target) return mid;
        if (mid > left && arr[mid - 1] == target) return mid - 1;
        if (mid < right && arr[mid + 1] == target) return mid + 1;
        
        // Aggressive Space Pruning
        if (target < arr[mid]) {
            right = mid - 2;
        } else {
            left = mid + 2;
        }
    }
    
    return -1;
}
```

> [!tip]
> The boundary checks `mid > left` and `mid < right` during the triad inspection are absolutely critical. Without them, checking `mid - 1` or `mid + 1` can fatally trigger an out-of-bounds memory access.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(\log N)$. Despite checking three elements per iteration, the search space is effectively halved (and pruned even faster by subtracting/adding 2), maintaining the strict logarithmic bound.
- **Space Complexity:** $\mathcal{O}(1)$. The algorithm requires zero auxiliary memory allocations.
