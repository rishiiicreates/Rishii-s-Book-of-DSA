---
type: concept
tags: [stack, cpp]
date: 2026-06-30
---
# Max of 1s

## Problem Statement
Given a binary matrix, find the area of the largest rectangle containing only 1s.

## Approach / Intuition
This can be modeled as finding the [[Largest Rectangle in a Histogram]] iteratively for each row. We can treat each row as the base of a histogram and update the heights row by row. If the current cell is 1, the height increases by 1; if 0, the height resets to 0.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N * M) where N is the number of rows and M is the number of columns.
- **[[Space Complexity]]:** O(M) to store the heights of the histogram for the current row and the stack.

## Sample Code
```cpp
int maxArea(int M[1000][1000], int n, int m) {
    vector<int> hist(m, 0);
    int max_area = 0;
    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            if(M[i][j] == 1) hist[j]++;
            else hist[j] = 0;
        }
        stack<int> st;
        for (int j = 0; j <= m; j++) {
            while (!st.empty() && (j == m || hist[st.top()] >= hist[j])) {
                int height = hist[st.top()];
                st.pop();
                int width = st.empty() ? j : j - st.top() - 1;
                max_area = max(max_area, height * width);
            }
            st.push(j);
        }
    }
    return max_area;
}
```

## New Keywords / STL Used
- `std::stack`, `std::vector`, `std::max`

## Edge Cases
- Matrix consisting entirely of 0s.
- Matrix consisting entirely of 1s.
- 1D matrix (single row or single column).
