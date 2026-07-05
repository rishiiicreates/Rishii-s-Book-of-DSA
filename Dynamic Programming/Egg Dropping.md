# Super Egg Drop

## Problem Statement
- given `k` eggs and `n` floors, find the minimum number of attempts needed to find the critical floor from which an egg drops and breaks.

## Approach / Intuition
- instead of standard DP which is O(K*N^2), we reframe the [[Dynamic Programming]] state: `dp[m][k]` represents the maximum number of floors we can check with `m` moves and `k` eggs. The transition is `dp[m][k] = dp[m-1][k-1] + dp[m-1][k] + 1` (floors below if it breaks + floors above if it doesn't + the floor we drop from). We iterate `m` until `dp[m][k] >= n`.

## Time & Space Complexity
- **[[time Complexity]]:** O(K \times \log N)
- **[[space Complexity]]:** O(K)

## Sample Code
```cpp
#include <vector>

using namespace std;

int superEggDrop(int k, int n) {
    vector<int> dp(k + 1, 0);
    int m = 0;
    
    while (dp[k] < n) {
        m++;
        for (int i = k; i > 0; --i) {
            dp[i] = dp[i - 1] + dp[i] + 1;
        }
    }
    
    return m;
}
```

## New Keywords / STL Used
- none specific.

## Edge Cases
- 1 egg, 1 floor, large number of floors.

NEXT: [[Index]]
