---
type: concept
tags: [array, cpp, sorting, linear-scan]
date: 2026-07-01
---
# Check if Array is Sorted

## Problem Statement
Given an array of $N$ integers, determine if the array is sorted in non-decreasing order.
*Example:* `[1, 2, 2, 3, 5]` returns `true`. `[1, 2, 4, 3, 5]` returns `false`.

---

## Approach: Linear Adjacent Comparison

To verify if an array is sorted in non-decreasing order, the mathematical condition $A[i-1] \le A[i]$ must hold true for all $1 \le i < N$.

Instead of checking all possible pairs, we only need to perform a single [[Linear Search]] traversal, comparing adjacent elements. If we find any adjacent pair where the strict inequality $A[i-1] > A[i]$ is true, it mathematically proves the array is not sorted, and we can instantly return `false`.

If the loop finishes without finding any such violation, the array is guaranteed to be sorted.

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

bool isSorted(const vector<int>& arr) {
    int n = arr.size();
    
    // Iterate from the second element
    for (int i = 1; i < n; ++i) {
        // If the previous element is strictly greater, it's not sorted
        if (arr[i - 1] > arr[i]) {
            return false;
        }
    }
    
    return true; // No violations found
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ where $N$ is the number of elements in the array. In the worst case (when the array is sorted or the violation is at the very end), we check $N-1$ pairs.
- **Space Complexity:** $O(1)$ auxiliary space, as we only use an iterator variable.

> [!note]
> **Edge Cases Handled:** 
> - **Empty Arrays / Single Element:** If $N \le 1$, the loop condition `i < n` immediately fails, and the function correctly returns `true`.
> - **Identical Elements:** The non-decreasing condition `A[i-1] <= A[i]` correctly handles arrays like `[2, 2, 2]`.
