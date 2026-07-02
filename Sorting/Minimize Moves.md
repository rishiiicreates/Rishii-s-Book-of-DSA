---
type: concept
tags: [sorting, cpp, math, median]
date: 2026-06-30
---
# Minimize Moves

## Problem Statement
Given an integer array `nums` of size $N$, find the minimum number of moves required to make all array elements equal. A move consists of strictly incrementing or decrementing an element by $1$.
Mathematically, find an integer $X$ that minimizes the sum of absolute differences: $f(X) = \sum_{i=0}^{N-1} |nums[i] - X|$.

---

## Approach: Mathematical Median Minimization

To minimize the total $L_1$ distance (sum of absolute differences), the optimal target value $X$ is mathematically proven to be the **median** of the array.
Why the median and not the mean?
The mean minimizes the sum of *squared* differences ($L_2$ distance). The median minimizes the sum of *absolute* differences. If you move $X$ away from the median, you strictly increase the absolute distance to the majority of elements.

Algorithm:
1. Apply [[Sorting]] to the array to deterministically find the median.
2. The median is located at index $\lfloor N / 2 \rfloor$.
3. Compute the sum of $|nums[i] - \text{median}|$ for all elements.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;

int minMoves2(vector<int>& nums) {
    if (nums.empty()) return 0;
    
    // Sort the array to find the true mathematical median
    sort(nums.begin(), nums.end());
    
    int n = nums.size();
    int median = nums[n / 2];
    int moves = 0;
    
    for (int num : nums) {
        moves += abs(num - median);
    }
    
    return moves;
}
```

> [!tip]
> If $N$ is even, there are two statistical medians: $\frac{N}{2} - 1$ and $\frac{N}{2}$. Mathematically, any integer $X$ within the continuous range $[\text{nums}[\frac{N}{2} - 1], \text{nums}[\frac{N}{2}]]$ yields the exact same minimum absolute difference. Thus, blindly picking $N/2$ is completely valid and optimal.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N \log N)$ mandated by the sorting operation. The subsequent accumulation scan takes $\mathcal{O}(N)$. Note that this can theoretically be optimized to $\mathcal{O}(N)$ using QuickSelect (`std::nth_element`).
- **Space Complexity:** $\mathcal{O}(\log N)$ auxiliary space required for the Introsort call under the hood of `std::sort`.
