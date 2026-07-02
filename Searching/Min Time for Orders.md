---
type: concept
tags: [searching, binary-search, cpp, math]
date: 2026-06-30
---
# Min Time to Complete Orders

## Problem Statement
Given an array `ranks` of size $N$ representing the rank of $N$ cooks, and an integer $P$ representing the number of parathas (orders) to be cooked.
A cook with rank $R$ cooks the $x$-th paratha in $R \times x$ minutes. 
(i.e., the 1st takes $R \times 1$, 2nd takes $R \times 2$, etc. Total time for $x$ parathas is $R \times \frac{x(x+1)}{2}$).
Find the minimum time required to cook all $P$ parathas.

---

## Approach: Parametric Binary Search with Quadratic Inverse

The function $f(T)$—"Can we cook $P$ parathas in $T$ minutes?"—is strictly monotonically increasing. We can binary search on the time space.
1. The lower bound is $0$.
2. The upper bound is the worst-case time: if the worst cook (or the only cook) cooked all $P$ parathas. Max rank is $10$, so $10 \times \frac{P(P+1)}{2}$. (Typically $10^{14}$ is a safe overarching upper bound).

For a candidate time $T$:
How many parathas can a cook with rank $R$ cook in $\le T$ minutes?
$R \times \frac{x(x+1)}{2} \le T \implies x^2 + x - \frac{2T}{R} \le 0$
Solving the quadratic equation for the positive root $x$:
$$x = \left\lfloor \frac{\sqrt{1 + \frac{8T}{R}} - 1}{2} \right\rfloor$$
We sum the roots $x$ for all cooks. If the sum $\ge P$, time $T$ is feasible.

---

## Code Implementation

```cpp
#include <vector>
#include <cmath>
using namespace std;

bool isPossible(const vector<int>& ranks, long long time, int orders) {
    long long parathasCooked = 0;
    
    for (int r : ranks) {
        // Solving quadratic equation R * x(x+1)/2 <= time
        long long maxParathasForCook = (sqrt(1 + 8.0 * time / r) - 1) / 2;
        parathasCooked += maxParathasForCook;
        
        if (parathasCooked >= orders) return true;
    }
    
    return parathasCooked >= orders;
}

long long minTime(const vector<int>& ranks, int orders) {
    long long low = 0;
    // Theoretical worst case: rank 10 cooks all 10^5 parathas
    // 10 * (10^5 * (10^5 + 1)) / 2 approx 5 * 10^10. 1e14 is a very safe upper bound.
    long long high = 1e14; 
    long long ans = high;
    
    while (low <= high) {
        long long mid = low + (high - low) / 2;
        
        if (isPossible(ranks, mid, orders)) {
            ans = mid;
            high = mid - 1; // Minimize the time
        } else {
            low = mid + 1;
        }
    }
    
    return ans;
}
```

> [!tip]
> Using the quadratic formula `sqrt(...)` transforms an inner $\mathcal{O}(P)$ loop (simulating the cooking process incrementally) into an $\mathcal{O}(1)$ mathematical calculation, drastically optimizing the validation function.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N \log(\text{Max Time}))$. Evaluating the quadratic equation takes $\mathcal{O}(1)$ time per cook, making the validation $\mathcal{O}(N)$. The binary search executes roughly $\approx 47$ iterations over $10^{14}$.
- **Space Complexity:** $\mathcal{O}(1)$. The evaluation operates purely on constant-space mathematical reductions.
