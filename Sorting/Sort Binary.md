---
type: concept
tags: [sorting, cpp, arrays, two-pointers]
date: 2026-06-30
---
# Sort Binary Array

## Problem Statement
Given a binary array strictly containing only $0$s and $1$s, sort the array in linear time $\mathcal{O}(N)$ and strictly $\mathcal{O}(1)$ auxiliary space.

---

## Approach: Dutch National Flag Degeneration (Two-Pointer Convergence)

Since the domain of values is strictly $\{0, 1\}$, applying standard $\mathcal{O}(N \log N)$ [[Sorting]] is a massive computational waste. 
This is a mathematically degenerate case of the Dutch National Flag problem (where colors = 2).

Algorithm:
1. Maintain two converging boundary pointers: `left` starting at index $0$, `right` starting at index $N-1$.
2. The invariant is that everything strictly before `left` is `0`, and everything strictly after `right` is `1`.
3. If `arr[left] == 0`, the invariant holds; increment `left`.
4. If `arr[right] == 1`, the invariant holds; decrement `right`.
5. If `arr[left] == 1` and `arr[right] == 0`, we have found a mathematical inversion. Swap them, increment `left`, and decrement `right`.
6. Terminate when the boundaries cross (`left >= right`).

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

void sortBinary(vector<int>& arr) {
    int left = 0;
    int right = arr.size() - 1;
    
    while (left < right) {
        if (arr[left] == 0) {
            left++;
        } 
        else if (arr[right] == 1) {
            right--;
        } 
        else {
            // Mathematical inversion isolated: swap to resolve
            swap(arr[left], arr[right]);
            left++;
            right--;
        }
    }
}
```

> [!tip]
> A simpler but non-swapping alternative is Counting Sort: count the number of zeroes, then overwrite the first $K$ elements with $0$ and the remaining with $1$. However, the two-pointer approach performs fewer writes on average, making it mathematically superior for cache coherence.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. The `left` and `right` pointers strictly converge, processing each array element exactly once.
- **Space Complexity:** $\mathcal{O}(1)$. Evaluated strictly in-place with zero dynamic memory overhead.
