# House Robber

## Problem Statement
- given an integer array representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police. Adjacent houses have security systems connected, so you cannot rob two adjacent houses.

## Approach / Intuition
- at any house $i$, we have two choices: rob it and add its money to the maximum money from house $i-2$, or skip it and keep the maximum money from house $i-1$. This presents a clear [[Dynamic Programming]] recurrence: `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`. We can optimize space by only storing the results of the previous two houses.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$ where $N$ is the number of houses.
- **[[space Complexity]]:** $O(1)$ space optimization.

## Sample Code
```cpp
#include <vector>
#include <algorithm>

int rob(std::vector<int>& nums) {
    int prev2 = 0;
    int prev1 = 0;
    for (int num : nums) {
        int temp = prev1;
        prev1 = std::max(prev2 + num, prev1);
        prev2 = temp;
    }
    return prev1;
}
```

## New Keywords / STL Used
- `std::max`

## Edge Cases
- empty array, single element array, all zero amounts.

NEXT: [[Index]]
