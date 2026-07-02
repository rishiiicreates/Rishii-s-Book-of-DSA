---
type: concept
tags: [searching, ternary-search, binary-search, cpp, math]
date: 2026-06-30
---
# Equalize Towers

## Problem Statement
Given an array of $N$ tower `heights` and an array of `costs` where `costs[i]` is the cost to increase or decrease the height of the $i$-th tower by $1$.
Find the minimum total cost to make all towers strictly equal in height.

---

## Approach: Cost Function Convexity (Ternary/Binary Search)

If we plot the total cost as a function of the target height $H$, the resulting function $f(H) = \sum_{i} \text{costs}[i] \times |H - \text{heights}[i]|$ is mathematically a **convex function** (specifically, a sum of absolute value functions, which forms a piece-wise linear convex curve).

Because the function strictly decreases to a global minimum and then strictly increases, we can find the minimum using **Binary Search on the Derivative** (comparing adjacent heights):
1. The search space for the optimal height is bounded between $\min(\text{heights})$ and $\max(\text{heights})$.
2. For a candidate height `mid`, calculate $f(\text{mid})$ and $f(\text{mid} + 1)$.
3. If $f(\text{mid}) < f(\text{mid} + 1)$, we are on the rising slope (right side) of the convex curve. The global minimum must be to the left, so `high = mid - 1`.
4. If $f(\text{mid}) \ge f(\text{mid} + 1)$, we are on the falling slope (left side). The global minimum must be to the right, so `low = mid + 1`.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;

long long computeCost(const vector<int>& heights, const vector<int>& costs, int targetHeight) {
    long long totalCost = 0;
    for (int i = 0; i < heights.size(); i++) {
        totalCost += (long long)costs[i] * abs(heights[i] - targetHeight);
    }
    return totalCost;
}

long long minEqualizationCost(const vector<int>& heights, const vector<int>& costs) {
    int low = *min_element(heights.begin(), heights.end());
    int high = *max_element(heights.begin(), heights.end());
    
    long long min_cost = computeCost(heights, costs, low);
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        long long costMid = computeCost(heights, costs, mid);
        long long costMidPlusOne = computeCost(heights, costs, mid + 1);
        
        min_cost = min({min_cost, costMid, costMidPlusOne});
        
        // Convex function slope analysis
        if (costMid < costMidPlusOne) {
            // Slope is positive, minimum is to the left
            high = mid - 1;
        } else {
            // Slope is negative, minimum is to the right
            low = mid + 1;
        }
    }
    
    return min_cost;
}
```

> [!warning]
> Be extremely careful with integer bounds. The `totalCost` can easily exceed the limits of a 32-bit signed integer. Casting `costs[i]` to `long long` before multiplying with the absolute difference is an absolute necessity.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N \log(\max(H) - \min(H)))$. Evaluating the cost takes $\mathcal{O}(N)$ time. The binary search halves the height bounds at each step.
- **Space Complexity:** $\mathcal{O}(1)$. The algorithm requires strictly no auxiliary data structures.
