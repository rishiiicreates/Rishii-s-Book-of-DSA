# Largest Submatrix

## Problem Statement
- given a 2D [[Matrix]] of integers, find the contiguous submatrix with the largest sum.

## Approach / Intuition
- we can solve this efficiently by fixing the left and right boundaries (columns) of the submatrix. For a fixed pair of left and right columns, we calculate the sum of each row between these columns. We can compress these row sums into a 1D array. Once we have this 1D array, we apply [[Kadane's Algorithm]] to find the maximum contiguous sum within it, representing the max submatrix sum for those column boundaries.

## Time & Space Complexity
- **[[time Complexity]]:** $O(C^2 \times R)$
- **[[space Complexity]]:** $O(R)$ for the temporary 1D array

## Sample Code
```cpp
#include <vector>
#include <algorithm>
#include <climits>

int kadane(const std::vector<int>& arr) {
    int max_so_far = arr[0];
    int current_max = arr[0];
    for (int i = 1; i < arr.size(); i++) {
        current_max = std::max(arr[i], current_max + arr[i]);
        max_so_far = std::max(max_so_far, current_max);
    }
    return max_so_far;
}

int maxSumSubmatrix(std::vector<std::vector<int>>& matrix) {
    if (matrix.empty() || matrix[0].empty()) return 0;
    
    int rows = matrix.size();
    int cols = matrix[0].size();
    int max_sum = INT_MIN;
    
    for (int left = 0; left < cols; left++) {
        std::vector<int> temp(rows, 0);
        
        for (int right = left; right < cols; right++) {
            for (int i = 0; i < rows; i++) {
                temp[i] += matrix[i][right];
            }
            
            int current_sum = kadane(temp);
            max_sum = std::max(max_sum, current_sum);
        }
    }
    
    return max_sum;
}
```

## New Keywords / STL Used
- `iNT_MIN`, `std::max`

## Edge Cases
- matrix with all negative numbers
- 1d matrix (single row or column)
- empty matrix

NEXT: [[Index]]
