---
type: concept
tags: [two-pointer, cpp, array, sorting]
date: 2026-06-30
---
# Count Triplets with Sum Smaller Than a Given Value

## Problem Statement
Given an array of distinct integers and a sum value, find the count of triplets with a sum smaller than the given value.

## Approach / Intuition
Sort the [[Array]] first to enable efficient searching. Iterate through the array, fixing the first element. Then, use a [[Two-Pointer]] setup (`left` and `right`) for the remainder of the array. If the sum of the triplet is less than the target, it implies that all pairs formed by the current `left` and any element between `left` and `right` will also yield a valid triplet, so we add `right - left` to the count and increment `left`. Otherwise, decrement `right`.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N^2)$
- **[[Space Complexity]]:** $O(1)$ auxiliary

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

int countTriplets(vector<int>& arr, int sum) {
    sort(arr.begin(), arr.end());
    int n = arr.size();
    int count = 0;
    
    for (int i = 0; i < n - 2; i++) {
        int left = i + 1, right = n - 1;
        while (left < right) {
            if (arr[i] + arr[left] + arr[right] < sum) {
                count += (right - left);
                left++;
            } else {
                right--;
            }
        }
    }
    return count;
}
```

## New Keywords / STL Used
`sort`

## Edge Cases
No such triplets exist, array contains negative numbers, array size less than 3.
