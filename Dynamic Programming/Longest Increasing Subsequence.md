---
type: concept
tags: [dp, cpp, lis]
date: 2026-06-30
---
# Longest Increasing Subsequence

## Problem Statement
Find the length of the longest strictly increasing subsequence in an array.

## Approach / Intuition
We can solve this using [[Dynamic Programming]] or binary search. The binary search approach maintains a `tails` array where `tails[i]` stores the smallest tail of all increasing subsequences of length `i+1`. We iterate through the array and use `std::lower_bound` to find the position to replace or extend the sequence.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log N)
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

int lengthOfLIS(vector<int>& nums) {
    vector<int> tails;
    for (int num : nums) {
        auto it = lower_bound(tails.begin(), tails.end(), num);
        if (it == tails.end()) {
            tails.push_back(num);
        } else {
            *it = num;
        }
    }
    return tails.size();
}
```

## New Keywords / STL Used
`std::lower_bound`

## Edge Cases
Empty array, single element, array with all identical elements, strictly decreasing array.
