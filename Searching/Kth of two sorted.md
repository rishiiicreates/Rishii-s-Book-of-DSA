# Kth Element of Two Sorted Arrays

## Problem Statement
- given two sorted arrays $A$ and $B$ of sizes $N$ and $M$ respectively, and an integer $K$, find the mathematically exact $K$-th smallest element when both arrays are combined.


## Approach: Partitioning via Binary Search

- the problem is a generalized form of finding the Median of Two Sorted Arrays. We want to partition the arrays such that there are exactly $K$ elements on the left side of the cuts.

- let the cut in $A$ be $i$ and the cut in $B$ be $j$. Thus, $i + j = K$.
- this partition is valid if:
- $a[i-1] \le B[j]$
- $b[j-1] \le A[i]$

- force $A$ to be the smaller array to guarantee $O(\log(\min(N, M)))$ performance.
- the search boundaries for the cut $i$ in $A$ are extremely strict:
   - minimum bound $L$: If $K > M$, we *must* take at least $K - M$ elements from $A$. Thus $L = \max(0, K - M)$.
   - maximum bound $R$: We can at most take all $N$ elements from $A$, or all $K$ required elements. Thus $R = \min(K, N)$.
- while $L \le R$:
   - for $i = L + \lfloor (R - L) / 2 \rfloor$ and $j = K - i$, extract bounds $l_1, l_2, r_1, r_2$ mapping missing indices to $\pm\infty$.
   - if $l_1 \le r_2$ and $l_2 \le r_1$, the partition is perfect. The $K$-th element is strictly the largest element on the left side: $\max(l_1, l_2)$.
   - if $l_1 > r_2$, the cut in $A$ is too far right. Search left ($R = i - 1$).
   - if $l_2 > r_1$, the cut in $A$ is too far left. Search right ($L = i + 1$).


## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>

using namespace std;

int kthElement(const vector<int>& a, const vector<int>& b, int k) {
    int n = a.size();
    int m = b.size();
    
    // Always binary search on the smaller array
    if (n > m) return kthElement(b, a, k);
    
    // Rigorous mathematical bounds for the binary search
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
    
    return -1; // Unreachable for valid mathematical inputs
}
```


## Complexity Analysis
- **time Complexity:** $O(\log(\min(N, M)))$. Binary search runs exclusively on the bounded space of the smaller array.
- **space Complexity:** $O(1)$ auxiliary space.

> [!tip]
> **Boundary Bounds:** The initialization of `low = max(0, k - m)` is the critical mathematical distinction between finding the Median and finding the $K$-th element. Without this constraint, `cut2` could result in a negative index or an index exceeding $M$, leading to memory corruption.

NEXT: [[Index]]
