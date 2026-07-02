---
type: concept
tags: [two-pointers, cpp, arrays, dutch-national-flag]
date: 2026-07-01
---
# Sort Colors (Dutch National Flag)

## Problem Statement
Given an array $A$ of $N$ objects containing discrete states $0$ (red), $1$ (white), and $2$ (blue), sort the objects in-place strictly in $O(N)$ time such that all identical colors are contiguous in the order $0, 1, 2$.

---

## Approach: 3-Way Partitioning (Dijkstra)

A standard sorting algorithm evaluates to $O(N \log N)$. Counting sort evaluates to $O(N)$ but demands two absolute passes over the sequence.
Edsger W. Dijkstra formulated the **Dutch National Flag Algorithm** to execute the exact permutation in a strict single pass.

We define three topological boundary pointers:
- $L$ (Low): Denotes the upper bound of the $0$ domain.
- $M$ (Mid): The exploratory cursor navigating the unpartitioned sequence.
- $H$ (High): Denotes the lower bound of the $2$ domain.

The array is partitioned into four dynamic domains:
1. $A[0 \dots L-1] = 0$
2. $A[L \dots M-1] = 1$
3. $A[M \dots H] = \text{Unexplored}$
4. $A[H+1 \dots N-1] = 2$

We iterate $M$ while $M \le H$:
- **If $A[M] == 0$:** Swap $A[L]$ and $A[M]$, increment both $L$ and $M$. (The element swapped into $M$ was previously behind $M$, thus it is strictly $1$, making it safe to increment $M$).
- **If $A[M] == 1$:** The element is structurally sound; increment $M$.
- **If $A[M] == 2$:** Swap $A[M]$ and $A[H]$, decrement $H$. (Do NOT increment $M$, because the element swapped from $H$ is unexplored and must be evaluated on the next cycle).

---

## Code Implementation

```cpp
#include <vector>
#include <utility>

using namespace std;

void sortColors(vector<int>& nums) {
    int low = 0;
    int mid = 0;
    int high = nums.size() - 1;
    
    // Evaluate unexplored sequence
    while (mid <= high) {
        if (nums[mid] == 0) {
            swap(nums[low], nums[mid]);
            low++;
            mid++;
        } else if (nums[mid] == 1) {
            mid++;
        } else { // nums[mid] == 2
            swap(nums[mid], nums[high]);
            high--;
        }
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ strict single-pass linear time.
- **Space Complexity:** $O(1)$ auxiliary memory.

> [!warning]
> **Mid Increment Asymmetry:** The crucial algorithmic trap lies in incrementing `mid`. When swapping with `low`, we can guarantee the incoming element is `1`, justifying `mid++`. When swapping with `high`, the incoming element is totally unknown, necessitating re-evaluation at the exact same `mid` index.
