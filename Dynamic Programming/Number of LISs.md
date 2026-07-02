---
type: concept
tags: [dp, cpp, lis]
date: 2026-06-30
---
# Number of Longest Increasing Subsequences

## Problem Statement
Find the total number of longest increasing subsequences in an integer array.

## Approach / Intuition
We use two [[Dynamic Programming]] arrays: `dp[i]` for the length of LIS ending at `i`, and `cnt[i]` for the number of such LISs ending at `i`. When we find an element `nums[j] < nums[i]`, if `dp[j] + 1 > dp[i]`, we update `dp[i]` and set `cnt[i] = cnt[j]`. If `dp[j] + 1 == dp[i]`, we add `cnt[j]` to `cnt[i]`.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N^2)
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
#include <vector>

using namespace std;

int findNumberOfLIS(vector<int>& nums) {
    int n = nums.size();
    if (n == 0) return 0;
    
    vector<int> dp(n, 1);
    vector<int> cnt(n, 1);
    int max_len = 1;
    
    for (int i = 1; i < n; ++i) {
        for (int j = 0; j < i; ++j) {
            if (nums[i] > nums[j]) {
                if (dp[j] + 1 > dp[i]) {
                    dp[i] = dp[j] + 1;
                    cnt[i] = cnt[j];
                } else if (dp[j] + 1 == dp[i]) {
                    cnt[i] += cnt[j];
                }
            }
        }
        max_len = max(max_len, dp[i]);
    }
    
    int ans = 0;
    for (int i = 0; i < n; ++i) {
        if (dp[i] == max_len) {
            ans += cnt[i];
        }
    }
    return ans;
}
```

## New Keywords / STL Used
`std::max`

## Edge Cases
Array with all identical elements, strictly decreasing array, multiple subsequences of the same max length.
