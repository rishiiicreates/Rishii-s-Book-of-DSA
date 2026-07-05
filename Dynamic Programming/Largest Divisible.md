# Largest Divisible Subset

## Problem Statement
- find the largest subset such that every pair `(i, j)` of elements satisfies `i % j == 0` or `j % i == 0`.

## Approach / Intuition
- first, sort the array. This transforms the problem into a variation of the [[Longest Increasing Subsequence]]. If we sort the array, for any new element `x` to be added to a divisible subset ending with `y`, we only need to check if `x % y == 0`. We maintain a `dp` array for the max subset size ending at each index, and a `parent` array to reconstruct the subset.

## Time & Space Complexity
- **[[time Complexity]]:** O(N^2)
- **[[space Complexity]]:** O(N)

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

vector<int> largestDivisibleSubset(vector<int>& nums) {
    int n = nums.size();
    if (n == 0) return {};
    
    sort(nums.begin(), nums.end());
    vector<int> dp(n, 1);
    vector<int> parent(n, -1);
    
    int max_idx = 0;
    
    for (int i = 1; i < n; ++i) {
        for (int j = 0; j < i; ++j) {
            if (nums[i] % nums[j] == 0 && dp[i] < dp[j] + 1) {
                dp[i] = dp[j] + 1;
                parent[i] = j;
            }
        }
        if (dp[i] > dp[max_idx]) {
            max_idx = i;
        }
    }
    
    vector<int> res;
    int curr = max_idx;
    while (curr != -1) {
        res.push_back(nums[curr]);
        curr = parent[curr];
    }
    
    return res;
}
```

## New Keywords / STL Used
- `std::sort`

## Edge Cases
- array of size 1, array with prime numbers, elements where none divides another.

NEXT: [[Index]]
