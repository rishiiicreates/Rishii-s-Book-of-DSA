# Target Sum

## Problem Statement
- given an integer array and a target, assign `+` or `-` to each integer so that the sum evaluates to the target. Return the number of different expressions that evaluate to target.

## Approach / Intuition
- let elements assigned `+` be $S_1$ and `-` be $S_2$. We know $S_1 - S_2 = target$ and $S_1 + S_2 = total\_sum$. Adding them gives $S_1 = (total\_sum + target) / 2$. This reduces the problem to [[Subset Count With Sum]] for a new target.

## Time & Space Complexity
- **[[time Complexity]]:** O(n * target)
- **[[space Complexity]]:** O(target)

## Sample Code
```cpp
int findTargetSumWays(vector<int>& nums, int target) {
    int sum = 0;
    for (int num : nums) sum += num;
    if (abs(target) > sum || (sum + target) % 2 != 0) return 0;
    
    int subset_sum = (sum + target) / 2;
    vector<int> dp(subset_sum + 1, 0);
    dp[0] = 1;
    
    for (int num : nums) {
        for (int i = subset_sum; i >= num; --i) {
            dp[i] += dp[i - num];
        }
    }
    return dp[subset_sum];
}
```

## New Keywords / STL Used
- `abs`, `vector`

## Edge Cases
- target absolute value greater than array sum, sum + target is odd, multiple zeros in input.

NEXT: [[Index]]
