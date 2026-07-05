# Minimum Cost to Cut a Board into Squares

## Problem Statement
- given a board of size M x N and costs associated with making horizontal and vertical cuts, find the minimum total cost to cut the board into single squares. The cost of a cut is its intrinsic cost multiplied by the number of segments it cuts through.

## Approach / Intuition
- we employ a [[Greedy]] strategy. Sort both the horizontal and vertical cut costs in descending order. Always perform the most expensive available cut first. A horizontal cut increases the number of vertical segments, and a vertical cut increases the number of horizontal segments.

## Time & Space Complexity
- **[[time Complexity]]:** O(M log M + N log N) for sorting the cut costs.
- **[[space Complexity]]:** O(1) auxiliary space.

## Sample Code
```cpp
#include <vector>
#include <algorithm>
#include <functional>

long long minCostToCut(std::vector<int>& X, std::vector<int>& Y) {
    std::sort(X.begin(), X.end(), std::greater<int>());
    std::sort(Y.begin(), Y.end(), std::greater<int>());
    
    int hzntl = 1, vert = 1;
    int i = 0, j = 0;
    long long totalCost = 0;
    
    while (i < X.size() && j < Y.size()) {
        if (X[i] > Y[j]) {
            totalCost += (long long)X[i] * vert;
            hzntl++;
            i++;
        } else {
            totalCost += (long long)Y[j] * hzntl;
            vert++;
            j++;
        }
    }
    
    while (i < X.size()) {
        totalCost += (long long)X[i] * vert;
        i++;
    }
    while (j < Y.size()) {
        totalCost += (long long)Y[j] * hzntl;
        j++;
    }
    
    return totalCost;
}
```

## New Keywords / STL Used
- `std::greater`

## Edge Cases
- a 1x1 board requires no cuts, completely symmetric costs.

NEXT: [[Index]]
