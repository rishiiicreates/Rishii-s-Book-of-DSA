# Split array

## Problem Statement
- determine if it is possible to split an array into two subarrays such that the sum of elements in the left subarray is equal to the sum of elements in the right subarray.

## Approach / Intuition
- this requires finding an index where the [[Prefix Sum]] up to that index equals the sum of the remaining elements. We first compute the total sum of the array. Then we iterate through the array, keeping a running sum. At each step, if the running sum is exactly half of the total sum, the array can be split evenly at this point.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$
- **[[space Complexity]]:** $O(1)$

## Sample Code
```cpp
#include <vector>
#include <numeric>

bool canSplitEqually(std::vector<int>& arr) {
    long long total = std::accumulate(arr.begin(), arr.end(), 0LL);
    
    if (total % 2 != 0) return false;
    
    long long running_sum = 0;
    for (int i = 0; i < arr.size() - 1; i++) {
        running_sum += arr[i];
        if (running_sum == total / 2) {
            return true;
        }
    }
    
    return false;
}
```

## New Keywords / STL Used
- `std::accumulate`

## Edge Cases
- odd total sum (impossible to split into integer sums)
- arrays with zeros or negative numbers leading to multiple valid split points
- empty or single-element arrays

NEXT: [[Index]]
