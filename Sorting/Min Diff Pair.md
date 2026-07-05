# Min Diff Pair

## Problem Statement
- given an array $A$ of $N$ integers ($N \ge 2$), find the minimum absolute mathematical difference between any two distinct elements.
- minimize $|A[i] - A[j]|$ for $i \ne j$.


## Approach: Bounded Adjacency post-Sort

- a brute-force combinatorial approach evaluates all $\frac{N(N-1)}{2}$ pairs, taking $O(N^2)$ time.
- however, we can exploit the metric geometry of numbers on a 1D line.

- if we sort the array $A$ such that $A[0] \le A[1] \le \dots \le A[N-1]$, the mathematical distance between any two elements $A[i]$ and $A[j]$ (where $i < j$) is exactly:
$$ |A[i] - A[j]| = (A[i+1] - A[i]) + (A[i+2] - A[i+1]) + \dots + (A[j] - A[j-1]) $$

- since all terms in the summation are non-negative, the distance between non-adjacent elements is strictly greater than or equal to the distance between the intermediate adjacent elements.
- therefore, the absolute global minimum difference **must** be realized by at least one pair of adjacent elements in the sorted array.
- we only need to evaluate $N-1$ adjacent differences.


## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>

using namespace std;

int findMinDiff(vector<int>& arr) {
    int n = arr.size();
    if (n < 2) return 0; // Mathematically undefined for N < 2
    
    // Sort array to collapse proximity
    sort(arr.begin(), arr.end());
    
    int minDiff = INT_MAX;
    
    // The global minimum must lie between adjacent elements in a sorted state
    for (int i = 1; i < n; i++) {
        minDiff = min(minDiff, arr[i] - arr[i - 1]);
    }
    
    return minDiff;
}
```


## Complexity Analysis
- **time Complexity:** $O(N \log N)$ dominated entirely by the sorting phase. The linear scan takes $O(N)$.
- **space Complexity:** $O(\log N)$ auxiliary space required for the Introsort stack.

> [!tip]
> **Duplicate Handling:** If the array contains duplicates, sorting places them adjacent to each other. The loop will evaluate $A[i] - A[i-1] = 0$, correctly recording $0$ as the absolute lowest possible minimum difference.

NEXT: [[Index]]
