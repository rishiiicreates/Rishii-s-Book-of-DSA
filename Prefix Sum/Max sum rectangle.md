# Max Sum Rectangle in a 2D Matrix

## Problem Statement
- given a 2D [[Matrix]], find the rectangle (submatrix) that yields the maximum possible sum.

## Approach / Intuition
- iterate by fixing the left and right column boundaries. Collapse the 2D elements between these boundaries into a 1D array representing the row-wise sum. Applying standard [[Kadane's Algorithm]] on this condensed 1D array reveals the optimal top and bottom boundaries, an elegant dimensional reduction powered by [[Prefix Sum]].

## Time & Space Complexity
- **[[time Complexity]]:** O(C^2 * R)
- **[[space Complexity]]:** O(R)

## Sample Code
```cpp
int maxSumRectangle(vector<vector<int>>& matrix) {
    if (matrix.empty()) return 0;
    int rows = matrix.size(), cols = matrix[0].size();
    int maxSum = INT_MIN;
    for (int left = 0; left < cols; left++) {
        vector<int> temp(rows, 0);
        for (int right = left; right < cols; right++) {
            for (int i = 0; i < rows; i++) {
                temp[i] += matrix[i][right];
            }
            int currentMax = temp[0], maxEndingHere = temp[0];
            for (int i = 1; i < rows; i++) {
                maxEndingHere = max(temp[i], maxEndingHere + temp[i]);
                currentMax = max(currentMax, maxEndingHere);
            }
            maxSum = max(maxSum, currentMax);
        }
    }
    return maxSum;
}
```

## New Keywords / STL Used
- `iNT_MIN`, `std::max`

## Edge Cases
- matrix with strictly all negative values, completely empty matrix.

NEXT: [[Index]]
