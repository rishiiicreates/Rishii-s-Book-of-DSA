---
type: concept
tags: [searching, binary-search, cpp, greedy, math]
date: 2026-06-30
---
# Maximize Median

## Problem Statement
Given an array of $N$ integers (where $N$ is odd) and an integer $K$, you can perform at most $K$ operations. Each operation consists of choosing an index $i$ and increasing `arr[i]` by $1$.
Return the maximum possible median of the array after performing at most $K$ operations.

---

## Approach: Parametric Binary Search + Greedy Validation

First, we sort the array. The median is located at index `mid_idx = N / 2`.
To maximize the median efficiently, we only care about increasing elements from `mid_idx` to $N - 1$. Increasing elements before `mid_idx` does absolutely nothing to improve the median.

The function $f(\text{target})$ determines if we can achieve a median of $\text{target}$ using $\le K$ operations. It is monotonically decreasing (if we can achieve $X$, we can achieve $X-1$).
1. Set `low = arr[N / 2]`.
2. Set `high = arr[N / 2] + K` (the absolute theoretical maximum if all $K$ operations were dumped onto the median and the elements to its right).
3. For a candidate median `mid`, calculate the cost:
   - For every $i$ from `mid_idx` to $N-1$, if `arr[i] < mid`, we must add `mid - arr[i]` to our operation cost.
   - If `cost > K` at any point, abort and return `false`.
4. Binary search exactly on this boolean boundary to find the maximum valid `mid`.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

bool isValidMedian(const vector<int>& arr, int n, long long k, long long targetMedian) {
    long long operationsNeeded = 0;
    
    // We only care about elements from the median onwards
    for (int i = n / 2; i < n; i++) {
        if (arr[i] < targetMedian) {
            operationsNeeded += (targetMedian - arr[i]);
            
            // Early pruning to prevent integer overflow and save time
            if (operationsNeeded > k) return false;
        }
    }
    return operationsNeeded <= k;
}

long long maxMedian(vector<int>& arr, long long k) {
    int n = arr.size();
    sort(arr.begin(), arr.end());
    
    long long low = arr[n / 2];
    long long high = arr[n / 2] + k;
    long long ans = low;
    
    while (low <= high) {
        long long mid = low + (high - low) / 2;
        
        if (isValidMedian(arr, n, k, mid)) {
            ans = mid;
            low = mid + 1; // Attempt to push the median even higher
        } else {
            high = mid - 1;
        }
    }
    
    return ans;
}
```

> [!important]
> The target median and the cost accumulators **must** use `long long`. The maximum theoretical median can easily overflow a standard 32-bit integer when large values of $K$ (e.g., $10^9$) are added to large array elements.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N \log N + N \log(\max(arr) + K))$. Sorting the array takes $\mathcal{O}(N \log N)$. The binary search takes $\log(\max(arr) + K)$ iterations, and each validation scan takes $\mathcal{O}(N)$ time.
- **Space Complexity:** $\mathcal{O}(\log N)$ required by the introsort call, otherwise $\mathcal{O}(1)$ auxiliary space.
