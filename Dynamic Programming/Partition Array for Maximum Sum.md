# Partition Array for Maximum Sum

## Problem Statement
- partition an integer array into contiguous subarrays of length at most `k`. After partitioning, each subarray has their values changed to become the maximum value of that subarray. Return the largest sum.

## Approach / Intuition
- use a 1D [[Dynamic Programming]] array where `dp[i]` is the maximum sum we can get for the prefix of length `i`. To calculate `dp[i]`, we iterate backwards up to `k` steps to form the last subarray. We keep track of the maximum element in this window and update `dp[i] = max(dp[i], dp[i-j] + max_val * j)` for `j` from 1 to `k`.

## Time & Space Complexity
- **[[time Complexity]]:** O(N \times K)
- **[[space Complexity]]:** O(N)

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

int maxSumAfterPartitioning(vector<int>& arr, int k) {
    int n = arr.size();
    vector<int> dp(n + 1, 0);
    
    for (int i = 1; i <= n; ++i) {
        int max_val = 0;
        for (int j = 1; j <= k && i - j >= 0; ++j) {
            max_val = max(max_val, arr[i - j]);
            dp[i] = max(dp[i], dp[i - j] + max_val * j);
        }
    }
    
    return dp[n];
}
```

## New Keywords / STL Used
- `std::max`

## Edge Cases
- `k == 1`, `k >= n`, array with all identical elements.

NEXT: [[Index]]
