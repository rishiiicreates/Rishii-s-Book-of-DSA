# Pick K (K-th Element of Two Sorted Arrays)

## Problem Statement
- given two sorted arrays $A$ and $B$ of sizes $N$ and $M$ respectively, find the mathematically exact $K$-th smallest element when both arrays are combined.


## Approach: Partitioning via Binary Search

- a naive merge algorithm would take $O(K)$ time. We can achieve $O(\log(\min(N, M)))$ by recognizing that picking the $K$-th element is equivalent to making a cut across both arrays such that exactly $K$ elements exist on the left side of the cut.

- assume we make a cut in $A$ at index $i$. Because we need exactly $K$ elements in total, the cut in $B$ must strictly be at index $j = K - i$.
- this partition is mathematically valid if:
- the largest element on the left of $A$ is $\le$ the smallest element on the right of $B$ (`l1 <= r2`).
- the largest element on the left of $B$ is $\le$ the smallest element on the right of $A$ (`l2 <= r1`).

- since the arrays are sorted, we can use binary search to find the correct index $i$ in the smaller array.
- force $A$ to be the smaller array to minimize the search space.
- initialize bounds for the cut in $A$: $L = \max(0, K - M)$ and $R = \min(K, N)$.
- while $L \le R$:
   - set $i = L + (R - L) / 2$ and $j = K - i$.
   - extract $l_1, r_1$ from $A$ and $l_2, r_2$ from $B$. Handle out-of-bounds indices by assigning $-\infty$ or $+\infty$.
   - if $l_1 \le r_2$ and $l_2 \le r_1$, the partition is perfect. The $K$-th element is $\max(l_1, l_2)$.
   - if $l_1 > r_2$, the cut in $A$ is too far right. Move left ($R = i - 1$).
   - else, the cut in $A$ is too far left. Move right ($L = i + 1$).


## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>

using namespace std;

int kthElement(const vector<int>& a, const vector<int>& b, int k) {
    int n = a.size(), m = b.size();
    
    // Always binary search on the smaller array for O(log(min(N,M)))
    if (n > m) return kthElement(b, a, k);
    
    // L cannot be less than 0, and must be at least K - M to ensure B isn't overdrawn.
    // R cannot exceed N, and cannot exceed K.
    int low = max(0, k - m);
    int high = min(k, n);
    
    while (low <= high) {
        int cut1 = low + (high - low) / 2;
        int cut2 = k - cut1;
        
        int l1 = (cut1 == 0) ? INT_MIN : a[cut1 - 1];
        int l2 = (cut2 == 0) ? INT_MIN : b[cut2 - 1];
        int r1 = (cut1 == n) ? INT_MAX : a[cut1];
        int r2 = (cut2 == m) ? INT_MAX : b[cut2];
        
        if (l1 <= r2 && l2 <= r1) {
            return max(l1, l2);
        }
        else if (l1 > r2) {
            high = cut1 - 1;
        }
        else {
            low = cut1 + 1;
        }
    }
    
    return 1; // Mathematically unreachable if K is valid
}
```


## Complexity Analysis
- **time Complexity:** $O(\log(\min(N, M)))$. We binary search strictly on the bounds of the smaller array.
- **space Complexity:** $O(1)$ auxiliary space.

> [!tip]
> **Boundary Bounds:** The initialization of `low` and `high` is mathematically critical. If $K$ is larger than $M$, we *must* take at least $K-M$ elements from $A$. Thus `low = max(0, K - M)`. Similarly, we can never take more elements from $A$ than either its total size $N$ or the requested $K$. Thus `high = min(K, N)`.

NEXT: [[Index]]
