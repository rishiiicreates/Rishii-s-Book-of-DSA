# Sorted Subsequence of 3

## Problem Statement
- given an array $A$ of $N$ integers, find a sorted subsequence of size 3. Mathematically, find three indices $i, j, k$ such that $0 \le i < j < k < N$ and $A[i] < A[j] < A[k]$.
- if multiple exist, return any one. If none exist, return an empty array.
- *example:* `[1, 2, 1, 1, 3]` returns `[1, 2, 3]`. `[5, 4, 3, 2, 1]` returns `[]`.


## Approach: Greedy State Tracking

- we want to find a sequence $A[i] < A[j] < A[k]$. We can greedily track the smallest element seen so far (`first`), and the smallest middle element seen so far (`second`).

- initialize `first` and `second` to $+\infty$.
- traverse the array element $x$:
   - if $x \le \text{first}$: Update `first` = $x$. This ensures we have the lowest possible starting point for a sequence.
   - else if $x \le \text{second}$: Update `second` = $x$. This ensures we have a valid $A[j] > A[i]$ that is as small as possible, maximizing the chance of finding an $A[k]$.
   - else ($x > \text{second}$): We have found an element strictly greater than `second`, which implies it is also strictly greater than `first`. The condition $A[i] < A[j] < A[k]$ is satisfied! Return the sequence.

- even if `first` gets updated *after* `second` was set (meaning they mathematically belong to different index orders), it does not invalidate the sequence! If we find an element $> \text{second}$, the *original* `first` that set `second` is implicitly still valid, preserving the relative index ordering constraint.


## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <limits>

using namespace std;

vector<int> find3Numbers(const vector<int>& arr) {
    int first = numeric_limits<int>::max();
    int second = numeric_limits<int>::max();
    
    for (int num : arr) {
        if (num <= first) {
            // Found a new smallest starting element
            first = num;
        } else if (num <= second) {
            // Found a valid middle element that is > first
            second = num;
        } else {
            // Found an element > second > first (using the implied original first)
            return {first, second, num};
        }
    }
    
    return {}; // No such subsequence found
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ as we only perform a single linear traversal of the array.
- **space Complexity:** $O(1)$ since only scalar state variables are required.

> [!tip]
> This pattern generalizes beautifully. If you needed to find a sorted subsequence of length $K$, you could maintain an array of $K-1$ smallest elements and use `lower_bound` (binary search) to update them, similar to the optimal solution for the **Longest Increasing Subsequence (LIS)** problem.

NEXT: [[Index]]
