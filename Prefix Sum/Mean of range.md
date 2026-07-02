---
type: concept
tags: [prefix sum, cpp, math, range-query]
date: 2026-06-30
---
# Mean of range

## Problem Statement
Given an array and multiple range queries $[L, R]$, find the mean (average) of the elements in each range efficiently.

## Approach / Intuition
To find the mean of a range, we need the sum of the elements in that range and the count of elements. The count is simply $R - L + 1$. We can compute the sum in $O(1)$ time by using a precomputed [[Prefix Sum Array]]. Then, we divide this sum by the count to get the mean. Floating point division is used to ensure precision.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$ for precomputation, $O(1)$ per query
- **[[Space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <vector>

class RangeMean {
private:
    std::vector<long long> prefix;
public:
    RangeMean(std::vector<int>& nums) {
        if (nums.empty()) return;
        prefix.resize(nums.size());
        prefix[0] = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            prefix[i] = prefix[i - 1] + nums[i];
        }
    }
    
    double getMean(int left, int right) {
        long long sum = (left == 0) ? prefix[right] : prefix[right] - prefix[left - 1];
        int count = right - left + 1;
        return (double)sum / count;
    }
};
```

## New Keywords / STL Used
None specific

## Edge Cases
- Range with a single element
- Ranges with negative values resulting in a negative mean
- Substantial sums requiring `long long` storage to avoid overflow
