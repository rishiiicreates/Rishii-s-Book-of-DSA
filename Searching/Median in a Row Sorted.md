---
type: concept
tags: [searching, binary search, cpp, matrix]
date: 2026-06-30
---
# Median in a Row Sorted Matrix

## Problem Statement
Given an $R \times C$ matrix where every row is sorted in strictly increasing order, find the overall median of the matrix. You may assume that $R \times C$ is always odd, ensuring a unique median element.

*Example:* 
$M = \begin{bmatrix} 1 & 3 & 5 \\ 2 & 6 & 9 \\ 3 & 6 & 9 \end{bmatrix}$
*Result:* $5$

---

## Approach: Binary Search on Value Range

Since the matrix is not completely sorted, a standard flatten-and-sort takes $O(RC \log(RC))$. We can optimize this by performing [[Binary Search]] on the range of possible numerical values.

1. The minimum possible element $L$ is the minimum of the first column.
2. The maximum possible element $R$ is the maximum of the last column.
3. The median of a dataset of size $N = R \times C$ is mathematically defined as the element which has exactly $\lfloor N / 2 \rfloor$ elements strictly smaller than or equal to it (in a 0-indexed sense). So we need an element with at least $\lceil N / 2 \rceil$ elements $\le$ it. Let `req = (R * C) / 2`.
4. For a candidate value `mid`, iterate through each row. Since each row is sorted, use `std::upper_bound` to count how many elements are $\le$ `mid`. Sum these counts.
5. If `count <= req`, the candidate `mid` is too small to be the median. Search the right half: `low = mid + 1`.
6. If `count > req`, `mid` could be the median, or the median is smaller. Search the left half: `high = mid - 1`.
7. At the end, `low` will pinpoint the exact median value.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

int median(const vector<vector<int>>& matrix) {
    int R = matrix.size();
    int C = matrix[0].size();
    
    // Find min and max bounds for binary search
    int low = 1e9, high = -1e9;
    for (int i = 0; i < R; i++) {
        low = min(low, matrix[i][0]);
        high = max(high, matrix[i][C - 1]);
    }
    
    int req = (R * C) / 2; // We need more than `req` elements <= median
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        int count = 0;
        
        // Count elements <= mid in each row
        for (int i = 0; i < R; i++) {
            count += upper_bound(matrix[i].begin(), matrix[i].end(), mid) - matrix[i].begin();
        }
        
        if (count <= req) {
            low = mid + 1; // Median must be larger
        } else {
            high = mid - 1; // Could be the median or median is smaller
        }
    }
    
    return low;
}
```

> [!important]
> Notice the usage of `std::upper_bound` which finds the first element strictly greater than `mid`. Subtracting the `begin()` iterator gives the exact count of elements $\le$ `mid`.

---

## Complexity Analysis
- **Time Complexity:** $O(R \log_2(\text{max} - \text{min}) \cdot \log_2 C)$. The binary search executes $\log(\text{Range})$ times. Each step loops $R$ times, doing an $O(\log C)$ binary search per row.
- **Space Complexity:** $O(1)$. No auxiliary space is used.
