# Triplet Sum

## Problem Statement
- given an array $A$ of $N$ integers and an integer $X$, find if there exists a triplet $(A[i], A[j], A[k])$ such that $A[i] + A[j] + A[k] = X$ and $i, j, k$ are strictly distinct indices.

- *example:* $A = [1, 4, 45, 6, 10, 8]$, $X = 22$
- *result:* `true` (Triplet is $4, 10, 8$)


## Approach: Fix and Find (Two Pointers)

- a naive cubic approach tests all $\binom{N}{3}$ combinations in $O(N^3)$ time. We can mathematically reduce this to $O(N^2)$ by applying the [[Two-Pointer]] paradigm nested inside a linear traversal.

- **monotonicity Requirement:** We first sort the array $A$ to enable the two-pointer convergence search.
- **fixation Loop:** Iterate through the array with an index `i` from $0$ to $N-3$. For each iteration, we logically "fix" $A[i]$ as the first element of our prospective triplet.
- **subproblem Reduction:** The problem mathematically collapses into finding a Pair Sum in the subarray $A[i+1 \dots N-1]$ that equals the new target $X - A[i]$.
- **convergence Search:** Initialize `left = i + 1` and `right = N - 1`. Check the sum $S = A[i] + A[\text{left}] + A[\text{right}]$.
   - if $S == X$, a valid triplet exists. Return `true`.
   - if $S < X$, increment `left`.
   - if $S > X$, decrement `right`.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

bool find3Numbers(vector<int>& arr, int X) {
    int n = arr.size();
    if (n < 3) return false;
    
    // Sort array to establish monotonicity
    sort(arr.begin(), arr.end());
    
    // Fix the first element
    for (int i = 0; i < n - 2; ++i) {
        int left = i + 1;
        int right = n - 1;
        
        // Two-pointer search in the remaining array
        while (left < right) {
            long long sum = (long long)arr[i] + arr[left] + arr[right];
            
            if (sum == X) {
                return true;
            } else if (sum < X) {
                left++;
            } else {
                right--;
            }
        }
    }
    
    return false;
}
```

> [!important]
> By fixing the smallest element in the triplet, we guarantee that we only search the subarray to its right (`left = i + 1`). This inherently prevents reusing elements and elegantly avoids exploring permutations of the same triplet.


## Complexity Analysis
- **time Complexity:** $O(N^2)$. Sorting takes $O(N \log N)$. The outer loop runs $O(N)$ times, and for each iteration, the inner two-pointer scan takes $O(N)$ time. The total time is strictly bounded by $O(N^2)$.
- **space Complexity:** $O(1)$ auxiliary space if in-place sorting is used.

NEXT: [[Index]]
