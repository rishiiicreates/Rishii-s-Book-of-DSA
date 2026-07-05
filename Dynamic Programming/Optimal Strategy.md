# Optimal Strategy for a Game

## Problem Statement
- consider a row of coins. Two players take turns picking a coin from either end. Find the maximum possible value the first player can collect, assuming both players play optimally.

## Approach / Intuition
- this requires interval [[Dynamic Programming]]. Let `dp[i][j]` be the max value a player can get from coins `i` to `j`. If we pick coin `i`, the opponent will leave us with `min(dp[i+2][j], dp[i+1][j-1])`. If we pick `j`, the opponent leaves us `min(dp[i+1][j-1], dp[i][j-2])`. Thus, `dp[i][j] = max(arr[i] + min(...), arr[j] + min(...))`.

## Time & Space Complexity
- **[[time Complexity]]:** O(N^2)
- **[[space Complexity]]:** O(N^2)

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

long long maximumAmount(int n, int arr[]) {
    vector<vector<long long>> dp(n, vector<long long>(n, 0));
    
    for (int len = 1; len <= n; ++len) {
        for (int i = 0; i <= n - len; ++i) {
            int j = i + len - 1;
            
            long long pick_i = arr[i];
            if (i + 2 <= j) pick_i += min(dp[i + 2][j], dp[i + 1][j - 1]);
            else if (i + 1 <= j) pick_i += 0;
            
            long long pick_j = arr[j];
            if (i <= j - 2) pick_j += min(dp[i + 1][j - 1], dp[i][j - 2]);
            else if (i <= j - 1) pick_j += 0;
            
            dp[i][j] = max(pick_i, pick_j);
        }
    }
    
    return dp[0][n - 1];
}
```

## New Keywords / STL Used
- `std::max`, `std::min`

## Edge Cases
- even vs odd number of coins, all coins of same value, array of size 2.

NEXT: [[Index]]
