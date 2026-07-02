---
type: concept
tags: [searching, binary-search, cpp, array, math]
date: 2026-06-30
---
# Minimum Days to Make M Bouquets

## Problem Statement
Given an integer array `bloomDay` where `bloomDay[i]` represents the day the $i$-th flower will bloom. 
You need to make $M$ bouquets. To make exactly one bouquet, you need to use $K$ **adjacent** flowers from the garden.
Return the minimum number of days you need to wait to be able to make $M$ bouquets from the garden. If it is impossible, return `-1`.

---

## Approach: Parametric Binary Search (Minimizing the Maximum)

The core insight is that the feasibility function $f(d)$—which returns `true` if $M$ bouquets can be made by day $d$, and `false` otherwise—is strictly monotonically increasing.
If it is possible to make $M$ bouquets on day $d$, it is mathematically guaranteed to be possible on day $d+1$. This monotonic boolean boundary $F, F, \dots, F, T, T, \dots, T$ perfectly permits binary search on the answer space.

Algorithm:
1. The lower bound `low` is the minimum day any flower blooms: $\min(\text{bloomDay})$.
2. The upper bound `high` is the maximum day any flower blooms: $\max(\text{bloomDay})$.
3. For a candidate day `mid`, simulate the bouquet creation:
   - Traverse `bloomDay`. If `bloomDay[i] <= mid`, increment an `adjacent` counter.
   - If `adjacent == K`, a bouquet is successfully formed: increment `bouquets` and reset `adjacent = 0`.
   - If `bloomDay[i] > mid`, the adjacency chain is broken: reset `adjacent = 0`.
4. If `bouquets >= M`, `mid` is a feasible answer, but we want the **minimum** day, so we search the left space: `high = mid - 1`.
5. Otherwise, `mid` is infeasible, we search the right space: `low = mid + 1`.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

bool possible(const vector<int>& bloomDay, int day, int m, int k) {
    int bouquets = 0;
    int adjacent = 0;
    
    for (int b : bloomDay) {
        if (b <= day) {
            adjacent++;
            if (adjacent == k) {
                bouquets++;
                adjacent = 0;
            }
        } else {
            adjacent = 0; // Contiguous chain broken
        }
    }
    return bouquets >= m;
}

int minDays(const vector<int>& bloomDay, int m, int k) {
    // Mathematical impossibility check
    long long requiredFlowers = (long long)m * (long long)k;
    if (requiredFlowers > bloomDay.size()) return -1;
    
    int low = *min_element(bloomDay.begin(), bloomDay.end());
    int high = *max_element(bloomDay.begin(), bloomDay.end());
    int ans = -1;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (possible(bloomDay, mid, m, k)) {
            ans = mid;         // Record feasible answer
            high = mid - 1;    // Optimize for a strictly smaller day
        } else {
            low = mid + 1;
        }
    }
    
    return ans;
}
```

> [!important]
> The multiplication $M \times K$ can easily exceed the 32-bit signed integer limit if both are large. You **must** cast them to `long long` before multiplication to prevent integer overflow in the impossibility check.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N \log(\max(D) - \min(D)))$, where $N$ is the number of flowers and $D$ is the bloomDay array. The search space is bounded by the difference between the maximum and minimum bloom days.
- **Space Complexity:** $\mathcal{O}(1)$. The algorithm evaluates the array strictly in-place.
