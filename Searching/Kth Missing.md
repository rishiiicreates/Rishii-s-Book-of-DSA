---
type: concept
tags: [searching, binary search, cpp]
date: 2026-06-30
---
# Kth Missing Positive Number

## Problem Statement
Given an array $A$ of strictly increasing positive integers, find the $K$-th positive integer that is missing from this sequence.

*Example:* $A = [2, 3, 4, 7, 11]$, $K = 5$
*Result:* $9$ (The missing sequence is $1, 5, 6, 8, 9, 10, \dots$)

---

## Approach: Binary Search on Missing Count (Optimal)

A linear scan could take $O(N)$ time. To optimize to $O(\log N)$, we can apply [[Binary Search]] by noticing a mathematical invariant:
At any index $i$, the number of missing positive integers preceding $A[i]$ is exactly $A[i] - (i + 1)$.

1. Use binary search to find the boundaries where the number of missing elements crosses $K$.
2. If `missing < K`, it means the $K$-th missing number is strictly to the right: `left = mid + 1`.
3. If `missing >= K`, the $K$-th missing number is to the left (or exactly before `mid`): `right = mid - 1`.
4. When the loop terminates, the $K$-th missing number can be calculated algebraically as `left + K` (or equivalently `A[right] + (K - missing_at_right)` which simplifies to `right + 1 + K = left + K`).

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

int findKthPositive(const vector<int>& arr, int k) {
    int left = 0;
    int right = arr.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        // Calculate how many positive integers are missing up to arr[mid]
        int missing = arr[mid] - (mid + 1);
        
        if (missing < k) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    // The target lies between arr[right] and arr[left]
    // Mathematically, the formula reduces to `left + k`
    return left + k;
}
```

> [!important]
> The math behind `left + k` is beautiful. The missing number occurs after `arr[right]`. 
> The number of missing elements up to `arr[right]` is $M = \text{arr}[\text{right}] - (\text{right} + 1)$.
> We need $(K - M)$ more missing elements starting from $\text{arr}[\text{right}]$.
> Thus, the result is $\text{arr}[\text{right}] + (K - M)$.
> Substituting $M$: $\text{arr}[\text{right}] + K - (\text{arr}[\text{right}] - \text{right} - 1) = \text{right} + 1 + K$.
> Since `left == right + 1` at the end of the `while` loop, this simplifies directly to `left + K`.

---

## Complexity Analysis
- **Time Complexity:** $O(\log_2 N)$. We perform a standard binary search over the array indices.
- **Space Complexity:** $O(1)$. No auxiliary space is used.
