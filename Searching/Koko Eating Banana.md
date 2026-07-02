---
type: concept
tags: [searching, binary search, cpp, math]
date: 2026-06-30
---
# Koko Eating Banana

## Problem Statement
Koko loves to eat bananas. There are $N$ piles of bananas, where the $i$-th pile has $P[i]$ bananas. The guards are gone for $H$ hours. Koko can decide her bananas-per-hour eating speed $K$. Each hour, she chooses a pile and eats $K$ bananas from it. If the pile has less than $K$ bananas, she eats all of them and will not eat any more bananas during this hour.

Find the minimum integer eating speed $K$ such that she can eat all the bananas within $H$ hours.

*Example:* $P = [3, 6, 7, 11]$, $H = 8$
*Result:* $4$

---

## Approach: Binary Search on Answer Space

The eating speed $K$ must lie in a finite range:
- **Minimum Speed ($L$):** $1$ (She must eat at least 1 banana per hour).
- **Maximum Speed ($R$):** $\max(P)$ (Eating faster than the largest pile does not save any more time because each pile takes at least 1 hour).

The time taken $T(K)$ to eat all bananas at speed $K$ is a monotonic decreasing function. Thus, we can apply [[Binary Search]].
1. Choose a candidate speed `mid`.
2. Compute the total hours required: $T(\text{mid}) = \sum_{i} \lceil P[i] / \text{mid} \rceil$.
3. If $T(\text{mid}) \le H$, it means Koko eats fast enough. We try to slow her down to find the minimum valid speed: `high = mid - 1`.
4. If $T(\text{mid}) > H$, Koko is eating too slowly. We must increase her speed: `low = mid + 1`.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
#include <cmath>
using namespace std;

long long getHours(const vector<int>& piles, int speed) {
    long long hours = 0;
    for (int pile : piles) {
        // Fast ceiling division for integers: ceil(a / b) = (a + b - 1) / b
        hours += (pile + speed - 1) / speed;
    }
    return hours;
}

int minEatingSpeed(const vector<int>& piles, int h) {
    int low = 1;
    int high = *max_element(piles.begin(), piles.end());
    int ans = high;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        long long hours = getHours(piles, mid);
        
        if (hours <= h) {
            ans = mid;
            high = mid - 1; // Try to minimize speed
        } else {
            low = mid + 1;  // Must eat faster
        }
    }
    
    return ans;
}
```

> [!important]
> Avoid floating-point arithmetic `std::ceil((double)a / b)` as it can lead to precision errors and is mathematically slower. Instead, use integer math: $\lceil a / b \rceil = (a + b - 1) / b$.

> [!warning]
> The `hours` calculation can exceed the 32-bit integer limit if the piles are very large and the speed is small. Always use `long long` for the accumulator.

---

## Complexity Analysis
- **Time Complexity:** $O(N \log_2(\max(P)))$. The search space is bounded by the maximum element, and each check takes $O(N)$ time.
- **Space Complexity:** $O(1)$. Auxiliary space is strictly constant.
