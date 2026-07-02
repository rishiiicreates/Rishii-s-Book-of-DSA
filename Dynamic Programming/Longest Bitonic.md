---
type: concept
tags: [dp, cpp, lis]
date: 2026-06-30
---
# Longest Bitonic Subsequence

## Problem Statement
Find the length of the longest subsequence that first increases and then decreases.

## Approach / Intuition
Compute the [[Longest Increasing Subsequence]] length ending at each index `i` from the left, stored in `inc[i]`. Then compute the LIS starting at each index `i` from the right, stored in `dec[i]`. The maximum value of `inc[i] + dec[i] - 1` across all valid bitonic points gives the length of the Longest Bitonic Subsequence. A valid bitonic sequence must have a length of at least 3, meaning `inc[i] > 1` and `dec[i] > 1`.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N^2)
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

int LongestBitonicSequence(vector<int> nums) {
    int n = nums.size();
    if (n < 3) return 0;
    
    vector<int> inc(n, 1);
    for (int i = 1; i < n; ++i) {
        for (int j = 0; j < i; ++j) {
            if (nums[i] > nums[j] && inc[i] < inc[j] + 1) {
                inc[i] = inc[j] + 1;
            }
        }
    }
    
    vector<int> dec(n, 1);
    for (int i = n - 2; i >= 0; --i) {
        for (int j = n - 1; j > i; --j) {
            if (nums[i] > nums[j] && dec[i] < dec[j] + 1) {
                dec[i] = dec[j] + 1;
            }
        }
    }
    
    int max_len = 0;
    for (int i = 0; i < n; ++i) {
        if (inc[i] > 1 && dec[i] > 1) {
            max_len = max(max_len, inc[i] + dec[i] - 1);
        }
    }
    
    return max_len;
}
```

## New Keywords / STL Used
`std::max`

## Edge Cases
Arrays strictly increasing or decreasing (no bitonic peak), array length less than 3, elements are all same.
