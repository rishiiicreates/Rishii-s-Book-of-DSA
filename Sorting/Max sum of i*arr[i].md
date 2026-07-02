---
type: concept
tags: [sorting, cpp, greedy, math]
date: 2026-06-30
---
# Maximize Sum of i * arr[i]

## Problem Statement
Given an array of $N$ integers, maximize the mathematical objective function:
$f(arr) = \sum_{i=0}^{N-1} (i \times arr[i])$
by reordering the elements of the array. The answer must be returned modulo $10^9 + 7$.

---

## Approach: Greedy Sorting Isomorphism

By the rearrangement inequality, the sum of products of two sequences is maximized when both sequences are sorted in the same monotonic direction (either both ascending or both descending).
Here, the sequence of indices $i$ is inherently strictly monotonically increasing: $0, 1, 2, \dots, N-1$.
Thus, to maximize the dot product of the indices and the array values, the array values must also be strictly monotonically increasing.

Algorithm:
1. Use [[Sorting]] to arrange the elements in ascending order.
2. Accumulate the sum $i \times arr[i]$.
3. Apply the modulo arithmetic property at every single accumulation step to strictly prevent integer overflow.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

int Maximize(vector<int>& arr) {
    const long long MOD = 1e9 + 7;
    
    // Satisfy the Rearrangement Inequality
    sort(arr.begin(), arr.end());
    
    long long maxSum = 0;
    for (int i = 0; i < arr.size(); i++) {
        // Cast to long long to prevent 32-bit multiplication overflow
        long long product = (1LL * arr[i] * i) % MOD;
        maxSum = (maxSum + product) % MOD;
    }
    
    return static_cast<int>(maxSum);
}
```

> [!important]
> The cast `1LL * arr[i] * i` is absolutely critical. If $arr[i] \approx 10^9$ and $i \approx 10^5$, their product is $10^{14}$, which catastrophic overflows a standard signed 32-bit integer (max $\approx 2 \times 10^9$) *before* the modulo can even be applied.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N \log N)$ driven entirely by the sorting operation. The evaluation scan is linear $\mathcal{O}(N)$.
- **Space Complexity:** $\mathcal{O}(\log N)$ required for the auxiliary recursion stack of `std::sort`.
