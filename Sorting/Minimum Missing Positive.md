# First Missing Positive

## Problem Statement
- given an unsorted integer array $A$ of size $N$, find the smallest missing positive integer. You must implement an algorithm that runs in $O(N)$ time and uses $O(1)$ auxiliary space.

- *example:* $A = [3, 4, -1, 1]$
- *result:* $2$


## Approach: Index Hash Mapping (Cycle Sort Variant)

- because we are looking for the *first* missing positive integer, the answer is mathematically bounded. It must lie in the range $[1, N+1]$.
- if the array contains exactly the numbers $1, 2, \dots, N$, the missing number is $N+1$.
- otherwise, the missing number is strictly within $1$ to $N$.

- we can use the array itself as a hash map. This is effectively a specialized version of [[Cycle Sort]].
- we want to place each positive integer $x \in [1, N]$ at its correct 0-indexed position: index $x - 1$. For example, $1$ goes to index $0$, $5$ goes to index $4$.
- iterate through the array. For the current element $A[i]$, if $1 \le A[i] \le N$ and it is not already at its correct index (i.e., $A[i] \neq A[A[i] - 1]$), we swap $A[i]$ with $A[A[i] - 1]$.
- because swapping brings a new element to $A[i]$, we use a `while` loop to continuously resolve the new element until it either falls out of range, is a duplicate, or lands in its correct spot.
- after resolving all elements, perform a final linear scan. The first index `i` where $A[i] \neq i + 1$ reveals the missing number $i + 1$.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

int firstMissingPositive(vector<int>& nums) {
    int n = nums.size();
    
    // Cycle-sort elements into their correct positions
    for (int i = 0; i < n; ++i) {
        while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
            swap(nums[i], nums[nums[i] - 1]);
        }
    }
    
    // Find the first index where the element is incorrect
    for (int i = 0; i < n; ++i) {
        if (nums[i] != i + 1) {
            return i + 1;
        }
    }
    
    // All integers from 1 to N are present
    return n + 1;
}
```

> [!warning]
> The condition `nums[nums[i] - 1] != nums[i]` is critical. It prevents an infinite loop when the array contains duplicate valid elements (e.g., $A = [1, 1]$).


## Complexity Analysis
- **time Complexity:** $O(N)$. Even though there is a `while` loop inside the `for` loop, each swap puts at least one element into its strictly correct final position. Thus, at most $N$ swaps occur across the entire execution.
- **space Complexity:** $O(1)$. The operations are performed completely in-place.

NEXT: [[Index]]
