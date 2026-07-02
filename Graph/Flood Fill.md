---
type: concept
tags: [graph, matrix, dfs, bfs, cpp]
date: 2026-06-30
---
# Flood Fill

## Problem Statement
An image is mathematically represented by an $M \times N$ integer matrix `image` where `image[i][j]` denotes the pixel value of the image.

Given a Cartesian coordinate $(sr, sc)$ representing the starting pixel (row and column) and a scalar `color`, execute a **Flood Fill** on the image.

*Definition:* A flood fill initiates at the starting pixel, plus any pixels 4-directionally topologically connected to the starting pixel of the exact identical original color, plus any pixels 4-directionally connected to those pixels (also of identical original color), and mathematically overwrites their scalar state to the new `color`.

---

## Approach: Conditional Connected Component Traversal

This is structurally identical to [[Number of Islands]], executing a single Cartesian BFS or DFS. However, instead of wiping all landmasses, we conditionally propagate state modifications *only* to adjacent cells whose topological values match the original target's state.

1. **Parameter Initialization:** Cache the absolute initial state `initialColor = image[sr][sc]`. If `initialColor == color`, the state mathematically satisfies the terminal condition identically, and traversing would induce an infinite topological loop without a visited array. Immediately return `image`.
2. **Radial Traversal:** Trigger a recursive DFS (or queue BFS) originating at `(sr, sc)`.
3. **Execution Block:**
   - Overwrite the current pixel: `image[r][c] = color`.
   - Iterate across all 4 delta coordinate vectors.
   - For each valid adjacent cell `(nr, nc)`: if `image[nr][nc] == initialColor`, recursively trigger traversal into that coordinate.
4. **Return:** The intrinsically mutated matrix.

---

## Code Implementation (DFS)

```cpp
#include <vector>
using namespace std;

// Cardinal Directional Vectors
const int dr[4] = {-1, 0, 1, 0};
const int dc[4] = {0, 1, 0, -1};

void dfsFill(vector<vector<int>>& image, int r, int c, int initialColor, int newColor) {
    int m = image.size();
    int n = image[0].size();
    
    // Mutate state to prevent recursive cyclic redundancy
    image[r][c] = newColor;
    
    for (int i = 0; i < 4; ++i) {
        int nr = r + dr[i];
        int nc = c + dc[i];
        
        // Strict boundary conditions and topological color validation
        if (nr >= 0 && nr < m && nc >= 0 && nc < n && image[nr][nc] == initialColor) {
            dfsFill(image, nr, nc, initialColor, newColor);
        }
    }
}

vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
    int initialColor = image[sr][sc];
    
    // Mathematical invariant check to prevent infinite cyclic stack overflow
    if (initialColor != color) {
        dfsFill(image, sr, sc, initialColor, color);
    }
    
    return image;
}
```

> [!warning]
> The absolute most critical failure point in Flood Fill algorithms is failing to trap the `initialColor == newColor` edge case. If the target area is already the target color, traversing strictly conditional on `image[r][c] == initialColor` will re-trigger infinitely across the same region, shattering the stack.

---

## Complexity Analysis
- **Time Complexity:** $O(M \times N)$. In the absolute worst-case scenario, the entire matrix shares the identical initial color and requires mutation, forcing the recursive stack to visit every cell exactly once.
- **Space Complexity:** $O(M \times N)$. The implicit dynamic overhead mapped to the maximum theoretical depth of the recursive execution stack in pathological structural layouts (e.g., long serpentine pixel chains).
