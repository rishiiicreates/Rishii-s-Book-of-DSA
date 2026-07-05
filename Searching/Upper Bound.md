# Upper Bound

## Problem Statement
- given a sorted array $A$ of size $N$ and a target value $X$, mathematically define and find the **Upper Bound** of $X$ in $A$.
- the upper bound is defined as the smallest index $i \in [0, N-1]$ such that $A[i] > X$.
- if no such element exists, the upper bound is conceptually at index $N$.


## Approach: Binary Search

- the Upper Bound strictly requires the element to be *greater* than the target (unlike Lower Bound, which allows equality). Since the array is monotonic, we can use **Binary Search**.

- initialize $L = 0$ and $R = N - 1$. Maintain a potential `ans` variable initialized to $N$.
- while $L \le R$:
   - calculate the midpoint $M = L + \lfloor (R - L) / 2 \rfloor$.
   - if $A[M] > X$: The element is strictly greater, satisfying the mathematical condition. We record `ans = M` and aggressively search the left half ($R = M - 1$) for an even smaller index.
   - if $A[M] \le X$: The element is less than or equal to the target. It does not satisfy the condition, so we must search the right half ($L = M + 1$).

- *note:* The C++ STL algorithm `std::upper_bound` provides an optimized implementation of this exact logic.


## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Manual Binary Search Implementation
int getUpperBoundManual(const vector<int>& nums, int target) {
    int n = nums.size();
    int low = 0, high = n - 1;
    int ans = n;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (nums[mid] > target) {
            ans = mid;         // Record valid answer
            high = mid - 1;    // Search left for a smaller index
        } else {
            low = mid + 1;     // Element <= target, search right
        }
    }
    
    return ans;
}

// STL Implementation
int getUpperBoundSTL(const vector<int>& nums, int target) {
    auto it = upper_bound(nums.begin(), nums.end(), target);
    return distance(nums.begin(), it);
}
```


## Complexity Analysis
- **time Complexity:** $O(\log N)$ due to the halving of the search space.
- **space Complexity:** $O(1)$ auxiliary space.

> [!note]
> **Difference from Lower Bound:** The *only* mathematical difference between Lower Bound and Upper Bound is the equality sign check. Lower Bound records the index when $A[M] \ge X$, while Upper Bound records the index when $A[M] > X$.

NEXT: [[Index]]
