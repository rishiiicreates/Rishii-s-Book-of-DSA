# Aggressive Cows

## Problem Statement
- farmer John has built a new long barn, with $N$ stalls located at positions given by an array $A$. He has $C$ aggressive cows. To prevent the cows from hurting each other, he wants to assign the cows to the stalls such that the minimum distance between any two of them is as large as possible.

- find the maximum possible minimum distance.

- *example:* $A = [1, 2, 8, 4, 9]$, $C = 3$
- *result:* $3$ (Place cows at $1, 4, 8$)


## Approach: Binary Search on the Answer

- this is a "max-min" optimization problem. Let the minimum distance between any two cows be $D$.
- **lower Bound ($L$):** $1$ (minimum possible distance if they are adjacent).
- **upper Bound ($R$):** $\max(A) - \min(A)$ (if there are only 2 cows, place them at the extremes).

- since we want to maximize the minimum distance, we can use [[Binary Search]] on $D$.
- **sort the array** $A$ so we can greedily place cows from left to right.
- for a candidate distance `mid`, place the first cow at $A[0]$.
- iterate through the array. Place the next cow at the first stall that is at least `mid` distance away from the previous placed cow.
- if we successfully place all $C$ cows, then `mid` is a valid distance. Since we want the *maximum* possible distance, we save this answer and search the right half: `low = mid + 1`.
- if we cannot place all cows, the required distance `mid` is too large. We search the left half: `high = mid - 1`.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

bool canPlaceCows(const vector<int>& stalls, int c, int dist) {
    int cowsPlaced = 1;
    int lastPos = stalls[0];
    
    for (int i = 1; i < stalls.size(); i++) {
        if (stalls[i] - lastPos >= dist) {
            cowsPlaced++;
            lastPos = stalls[i];
            if (cowsPlaced == c) return true;
        }
    }
    return false;
}

int aggressiveCows(vector<int>& stalls, int c) {
    sort(stalls.begin(), stalls.end());
    
    int low = 1;
    int high = stalls.back() - stalls.front();
    int ans = 1;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (canPlaceCows(stalls, c, mid)) {
            ans = mid;
            low = mid + 1; // Try to maximize the distance
        } else {
            high = mid - 1; // Distance is too large
        }
    }
    
    return ans;
}
```

> [!tip]
> The greedy choice works because sorting the array ensures that making the next valid choice as early as possible leaves the maximal remaining room for subsequent cows.


## Complexity Analysis
- **time Complexity:** $O(N \log N + N \log_2(\max(A) - \min(A)))$. Sorting takes $O(N \log N)$. The binary search executes $\log(\text{Range})$ times, and each check is an $O(N)$ linear scan.
- **space Complexity:** $O(1)$ auxiliary space, not counting the stack space used by `std::sort`.

NEXT: [[Index]]
