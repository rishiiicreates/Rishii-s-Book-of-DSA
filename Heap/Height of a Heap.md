# Height of a Heap

## Problem Statement
- find the height of a [[Heap]] given the number of nodes `N`.

## Approach / Intuition
- a [[Heap]] is a complete [[Binary Tree]]. For a complete binary tree with `N` nodes, the height `h` (where a tree with one node has height 0 or 1 depending on convention) can be determined using a logarithm. The standard approach states the height is `floor(log2(N))`.

## Time & Space Complexity
- **[[time Complexity]]:** O(1) mathematically, or O(log N) if iteratively calculated.
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
#include <cmath>

using namespace std;

int heapHeight(int N) {
    return log2(N);
}
```

## New Keywords / STL Used
- `log2`

## Edge Cases
- `n = 1` (Height is 0).
- very large `N`.

NEXT: [[Index]]
