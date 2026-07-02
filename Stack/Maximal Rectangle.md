---
type: concept
tags: [stack, monotonic-stack, dynamic-programming, cpp, geometry, math]
date: 2026-06-30
---
# Maximal Rectangle in 2D Binary Matrix

## Problem Statement
Given a discrete 2D binary matrix structured with strictly $0$s and $1$s, mathematically calculate the absolute maximum bounded geometrical area of a rectangle containing strictly only $1$s.

---

## Approach: Dynamic Histogram Projection

This 2D spatial challenge mathematically reduces to executing the **Largest Rectangle in Histogram** evaluation $\mathcal{O}(R)$ times, iteratively projecting the 2D grid structurally into $1D$ spatial histograms.

Algorithm:
1. Maintain an $\mathcal{O}(C)$ sized auxiliary vector `heights` representing the cumulative geometric height of contiguous $1$s for the current theoretical base row.
2. Iterate strictly over each row in the bounded matrix.
3. For each spatial column $j$, mathematically update the histogram structure:
   - If `matrix[i][j] == '1'`, algebraically increment `heights[j]`.
   - If `matrix[i][j] == '0'`, forcefully collapse `heights[j]` to $0$, as the contiguous spatial boundary is broken.
4. For the resolved histogram of row $i$, theoretically apply the $\mathcal{O}(C)$ LIFO bounding mechanism to extract the absolute maximal valid rectangle resting on that row.
5. Track the theoretical global geometric maximum.

---

## Code Implementation

```cpp
#include <vector>
#include <stack>
#include <algorithm>

using namespace std;

// Extracted monotonic constraint evaluation
long long largestRectangleArea(const vector<int>& heights) {
    int n = heights.size();
    stack<int> s;
    long long max_area = 0;
    
    for (int i = 0; i <= n; i++) {
        int current_height = (i == n) ? 0 : heights[i];
        
        while (!s.empty() && current_height < heights[s.top()]) {
            int bottleneck_index = s.top();
            s.pop();
            long long bottleneck_height = heights[bottleneck_index];
            int right_bound = i;
            int left_bound = s.empty() ? -1 : s.top();
            long long width = right_bound - left_bound - 1;
            max_area = max(max_area, bottleneck_height * width);
        }
        s.push(i);
    }
    return max_area;
}

int maximalRectangle(const vector<vector<char>>& matrix) {
    if (matrix.empty() || matrix[0].empty()) return 0;
    
    int rows = matrix.size();
    int cols = matrix[0].size();
    vector<int> heights(cols, 0);
    long long max_rect = 0;
    
    for (int i = 0; i < rows; i++) {
        // Dynamically project grid structures to linear histograms
        for (int j = 0; j < cols; j++) {
            if (matrix[i][j] == '1') {
                heights[j]++;
            } else {
                heights[j] = 0; // Absolute structural collapse
            }
        }
        
        // Evaluate theoretical geometric bounds for current base plane
        max_rect = max(max_rect, largestRectangleArea(heights));
    }
    
    return max_rect;
}
```

> [!tip]
> A highly optimized bit-masking dynamic programming variant exists utilizing left and right bounds specifically for Boolean spatial mapping. However, leveraging the Histogram transformation structurally preserves modularity and avoids complex edge-case evaluations.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(R \times C)$. Structurally updating the $1D$ histogram demands $\mathcal{O}(C)$ operations, and mathematically bounded evaluation utilizes strictly $\mathcal{O}(C)$ operations per row.
- **Space Complexity:** $\mathcal{O}(C)$ to maintain the temporal sequence constraints (the histogram state and auxiliary LIFO evaluation structures).
