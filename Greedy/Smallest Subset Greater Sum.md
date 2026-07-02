---
type: concept
tags: [greedy, cpp, smallest-subset-greater-sum]
date: 2026-06-30
---
# Smallest Subset Greater Sum

## Problem Statement
Find the minimum number of elements in a subset such that their sum is strictly greater than the sum of the remaining elements in the array.

## Approach / Intuition
Calculate the total sum of the array. Sort the array in descending order. Keep adding elements to the subset and keep track of the subset sum using a [[Greedy Algorithm]]. Stop when the subset sum is greater than half of the total sum.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log N)
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
#include <vector>
#include <numeric>
#include <algorithm>

using namespace std;

int minElements(vector<int>& arr) {
    long long totalSum = accumulate(arr.begin(), arr.end(), 0LL);
    sort(arr.begin(), arr.end(), greater<int>());
    long long currentSum = 0;
    int count = 0;
    for (int num : arr) {
        currentSum += num;
        count++;
        if (currentSum > totalSum / 2) {
            break;
        }
    }
    return count;
}
```

## New Keywords / STL Used
- `accumulate`, `greater<int>()`

## Edge Cases
- Array with all zeros
- Array with a single element
