# Rotate Array

## Problem Statement
- given an integer array `nums`, rotate the array to the right by `k` steps, where $k \ge 0$.
- this means the element at index $i$ moves to $(i + k) \pmod N$, where $N$ is the size of the array.


## Approach: Block Reversal Strategy

- the naive approach involves shifting the elements one by one $k$ times, which takes $\mathcal{O}(N \times k)$ time. An intermediate approach uses an extra array of size $N$, taking $\mathcal{O}(N)$ time and $\mathcal{O}(N)$ space.

- the **Block Reversal Strategy** relies on mathematical properties of array indices. Since rotating by $k$ when $k \ge N$ is equivalent to rotating by $k \pmod N$, we first effectively reduce $k$.
- the strategy requires three in-place reversals:
- **reverse the entire array:** This moves the last $k$ elements to the front, but their internal order is backwards.
- **reverse the first $k$ elements:** This restores the original internal order of the chunk that wrapped around.
- **reverse the remaining $N - k$ elements:** This restores the internal order of the rest of the array.

- this powerful mathematical property transforms the elements into their exactly desired rotated positions through simple $\mathcal{O}(1)$ space swaps.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

void rotateArray(vector<int>& arr, int k) {
    int n = arr.size();
    if (n == 0) return;
    
    // Crucial step: handle k larger than array size
    k = k % n;
    if (k == 0) return;
    
    // Step 1: Reverse the entire array
    reverse(arr.begin(), arr.end());
    
    // Step 2: Reverse the first k elements
    reverse(arr.begin(), arr.begin() + k);
    
    // Step 3: Reverse the remaining n - k elements
    reverse(arr.begin() + k, arr.end());
}
```

> [!tip]
> `std::reverse` from `<algorithm>` provides an optimized in-place reversal in C++. Under the hood, it performs a classic two-pointer swap from the outside towards the center.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. The first reverse takes $\mathcal{O}(N/2)$ swaps. The subsequent two take $\mathcal{O}(k/2)$ and $\mathcal{O}((N-k)/2)$ swaps respectively. Total operations scale linearly with $N$.
- **space Complexity:** $\mathcal{O}(1)$. All operations are done in-place without utilizing any auxiliary scaling memory.

NEXT: [[Index]]
