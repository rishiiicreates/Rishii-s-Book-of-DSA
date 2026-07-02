---
type: concept
tags: [two-pointer, cpp, greedy, geometry, math]
date: 2026-06-30
---
# Container With Most Water (Maximal Bounded Area)

## Problem Statement
Given an array of $N$ non-negative integers representing heights of vertical lines located at spatial integer coordinates $x = i$, mathematically compute the two lines that construct a two-dimensional geometric container holding the absolute maximum volume of water.

---

## Approach: Greedy Two-Pointer Bounding

The volume of any geometric container defined by lines at indices $L$ and $R$ evaluates strictly to:
$$ \text{Volume} = (R - L) \times \min(\text{height}[L], \text{height}[R]) $$

A combinatorial brute force evaluates all pairs in $\mathcal{O}(N^2)$. However, we can construct an optimal [[Two-Pointer]] bounding system starting at the maximum possible width:
1. Initialize $L = 0$ and $R = N - 1$.
2. Calculate the contained geometrical volume. Update the global absolute maximum.
3. **Mathematical Invariant:** The overall volume is fundamentally restricted by the *shorter* line. If we move the pointer corresponding to the taller line inward, the width strictly decreases, and the height can *never* increase beyond the current shorter line. Therefore, moving the taller line mathematically guarantees a reduction (or equality) in volume.
4. To seek a potentially larger volume, we must greedily move the pointer defining the shorter bound inward, sacrificing width in exchange for theoretically higher bounds.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

int maxArea(const vector<int>& height) {
    int maxWater = 0;
    int left = 0;
    int right = height.size() - 1;
    
    while (left < right) {
        // Geometric evaluation of bounded volume
        int currentHeight = min(height[left], height[right]);
        int currentWidth = right - left;
        
        maxWater = max(maxWater, currentHeight * currentWidth);
        
        // Greedily abandon the strict limiting boundary
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    
    return maxWater;
}
```

> [!tip]
> If `height[left] == height[right]`, advancing *either* pointer is mathematically valid. The bounded height is symmetrically bottlenecked by both sides; shrinking either side forces the container to require a theoretically taller subsequent line to eclipse the current max volume.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N)$. The spatial boundary converges linearly exactly once across the $N$ dimensions.
- **Space Complexity:** $\mathcal{O}(1)$. Evaluated strictly via bounded integer variables tracking geometrical states.
