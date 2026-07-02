---
type: concept
tags: [two-pointer, cpp, sum-problem]
date: 2026-06-30
---
# Find the Closest Pair from Two Sorted Arrays

## Problem Statement
Given two sorted arrays and a number `x`, find the pair consisting of one element from each array whose sum is closest to `x`.

## Approach / Intuition
Initialize one pointer at the start of the first array and another at the end of the second array. Calculate the sum. If the absolute difference with `x` is minimal, update the best pair. If the sum is smaller than `x`, advance the pointer in the first array to increase the sum. Otherwise, decrease the pointer in the second array. This [[Two Pointers]] strategy effectively searches the solution space.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n + m)
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
pair<int, int> closestPair(const vector<int>& arr1, const vector<int>& arr2, int x) {
    int m = arr1.size();
    int n = arr2.size();
    int left = 0;
    int right = n - 1;
    int minDiff = INT_MAX;
    pair<int, int> res = {-1, -1};
    while (left < m && right >= 0) {
        int sum = arr1[left] + arr2[right];
        int diff = abs(sum - x);
        if (diff < minDiff) {
            minDiff = diff;
            res = {arr1[left], arr2[right]};
        }
        if (sum < x) {
            left++;
        } else {
            right--;
        }
    }
    return res;
}
```

## New Keywords / STL Used
`std::abs`, `std::pair`

## Edge Cases
Empty arrays, exact sum exists, multiple pairs with the same minimum difference.
