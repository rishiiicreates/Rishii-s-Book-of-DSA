---
type: concept
tags: [dsa, cpp, range-query, tree]
date: 2026-06-30
---
# 3D Binary Indexed Tree / Segment Tree

## Problem Statement
Given a 3D matrix, support point updates (add a value to cell `[x][y][z]`) and region queries (sum of elements in subgrid `[x1..x2][y1..y2][z1..z2]`).

## Approach / Intuition
Extend the 1D [[Fenwick Tree]] to 3 dimensions by using nested loops. A 3D BIT handles point updates and queries prefix sums `[1..x][1..y][1..z]`. To get the sum of an arbitrary 3D subgrid, use the Inclusion-Exclusion principle across all 8 corners of the cuboid defined by the query.

## Time & Space Complexity
- **[[Time Complexity]]:** O(log X * log Y * log Z) per query/update
- **[[Space Complexity]]:** O(X * Y * Z)

## Sample Code
```cpp
// Pseudocode for 3D BIT update
/*
void add3D(int x, int y, int z, int val) {
    for (int i = x; i <= MAX_X; i += i & -i)
        for (int j = y; j <= MAX_Y; j += j & -j)
            for (int k = z; k <= MAX_Z; k += k & -k)
                tree[i][j][k] += val;
}
*/
```

## New Keywords / STL Used
None specific

## Edge Cases
Large matrix dimensions causing Memory Limit Exceeded, 0-indexing alignment.
