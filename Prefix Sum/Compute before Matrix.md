---
type: concept
tags: [prefix sum, cpp, 2d-matrix, inverse]
date: 2026-06-30
---
# Find Original Matrix from 2D Prefix Sum (Compute before Matrix)

## Problem Statement
Given a 2D prefix sum matrix computed traditionally, reconstruct and strictly return the original array.

## Approach / Intuition
The established prefix sum at `(i, j)` equates mathematically to `original[i][j] + prefix[i-1][j] + prefix[i][j-1] - prefix[i-1][j-1]`. By cleanly inverting this associative formula, we isolate the base value `original[i][j]`, completely reversing the 2D [[Prefix Sum]] footprint step-by-step.

## Time & Space Complexity
- **[[Time Complexity]]:** O(R * C)
- **[[Space Complexity]]:** O(R * C)

## Sample Code
```cpp
vector<vector<int>> computeOriginalMatrix(vector<vector<int>>& prefix) {
    if (prefix.empty()) return {};
    int rows = prefix.size(), cols = prefix[0].size();
    vector<vector<int>> res(rows, vector<int>(cols, 0));
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            int val = prefix[i][j];
            if (i > 0) val -= prefix[i-1][j];
            if (j > 0) val -= prefix[i][j-1];
            if (i > 0 && j > 0) val += prefix[i-1][j-1];
            res[i][j] = val;
        }
    }
    return res;
}
```

## New Keywords / STL Used
None

## Edge Cases
1x1 minimal matrix dimensions, universally zeroed arrays mirroring identically between origin and prefix.
