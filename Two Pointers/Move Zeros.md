---
type: concept
tags: [two-pointers, cpp, arrays, in-place]
date: 2026-07-01
---
# Move Zeroes

## Problem Statement
Given an integer array $A$ of size $N$, structurally mutate the array in-place such that all $0$ values are translated to the end of the array, while preserving the relative topological order of all non-zero elements.

---

## Approach: Slow/Fast Pointer State Machine

This problem fundamentally requires an $O(N)$ traversal with in-place mutation, necessitating a dual-index state machine.
We define two orthogonal pointers:
1. **Fast Pointer ($R$):** The exploratory cursor traversing every element in $A$ from $0$ to $N-1$.
2. **Slow Pointer ($L$):** The assignment boundary separating the partitioned non-zero domain from the unexplored sequence.

At any step, the sub-sequence $A[0 \dots L-1]$ contains exclusively non-zero elements in their original relative sequence.
When the Fast Pointer $R$ encounters a non-zero element:
- We mathematically transpose $A[L]$ and $A[R]$.
- We increment the boundary $L$.

If $R$ encounters a $0$, it skips and increments. By the end of the sequence, all non-zero elements are densely packed at the front, and the swapped $0$s are relegated to the tail.

---

## Code Implementation

```cpp
#include <vector>
#include <utility>

using namespace std;

void moveZeroes(vector<int>& nums) {
    int n = nums.size();
    int insert_pos = 0; // The slow boundary
    
    for (int i = 0; i < n; i++) {
        // If element is non-zero, transpose into the boundary
        if (nums[i] != 0) {
            swap(nums[insert_pos], nums[i]);
            insert_pos++;
        }
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ strict linear scan.
- **Space Complexity:** $O(1)$ auxiliary memory.

> [!tip]
> **Sub-optimal Write Inversions:** An alternative approach writes all non-zeroes to the front and explicitly fills the tail with zeroes. While mathematically equivalent, the swap approach scales better in domains where zeroes are sparse, preventing redundant linear trailing writes. However, if the array is dense with zeroes, the swap approach performs unnecessary transpositions of zeroes with zeroes, triggering unnecessary cache invalidations.
