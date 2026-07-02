---
type: concept
tags: [backtracking, cpp, array, math]
date: 2026-06-30
---
# Permutations

## Problem Statement
Given an array of distinct integers, return all the possible permutations.

## Approach / Intuition
Use [[Backtracking]] to generate all orderings. Maintain a `used` array to track which elements are currently in the permutation, or simply swap elements in-place within the [[Array]]. In the swapping approach, for each index, swap it with all subsequent indices, recurse, and then backtrack by swapping them back to restore the original state.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N \cdot N!)$
- **[[Space Complexity]]:** $O(N)$ for the recursion stack

## Sample Code
```cpp
#include <vector>

using namespace std;

void permuteHelper(vector<int>& nums, int start, vector<vector<int>>& res) {
    if (start == nums.size()) {
        res.push_back(nums);
        return;
    }
    for (int i = start; i < nums.size(); i++) {
        swap(nums[start], nums[i]);
        permuteHelper(nums, start + 1, res);
        swap(nums[start], nums[i]);
    }
}

vector<vector<int>> permute(vector<int>& nums) {
    vector<vector<int>> res;
    permuteHelper(nums, 0, res);
    return res;
}
```

## New Keywords / STL Used
`swap`

## Edge Cases
Array with a single element, empty array, array with duplicate elements (requires sorting and skipping duplicates).
