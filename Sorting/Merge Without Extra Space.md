# Merge Two Sorted Arrays Without Extra Space

## Problem Statement
- merge two strictly sorted arrays `arr1` (size $N$) and `arr2` (size $M$) completely in-place. After merging, `arr1` must contain the $N$ smallest elements in strictly sorted order, and `arr2` must contain the remaining $M$ elements in strictly sorted order. You must strictly use $\mathcal{O}(1)$ auxiliary space.


## Approach: The Gap Method (Shell Sort Derivative)

- standard merging requires $\mathcal{O}(N+M)$ auxiliary space. To merge in-place in $\mathcal{O}((N+M) \log(N+M))$, we exploit the Gap Method mathematically derived from [[Shell Sort]].

- the algorithm maintains a dynamic `gap` between compared elements.
- initial Gap: $\lceil (N + M) / 2 \rceil$.
- maintain two pointers, `left = 0` and `right = left + gap`.
- compare the elements at `left` and `right`. If `element(left) > element(right)`, swap them.
- the pointers iterate through the bounds. We must mathematically map the continuous abstract index to the specific array (`arr1` or `arr2`).
   - if index $< N$, it maps to `arr1[index]`.
   - if index $\ge N$, it maps to `arr2[index - N]`.
- once the pointers hit the end boundary, mathematically shrink the gap: $\text{gap} = \lceil \text{gap} / 2 \rceil$.
- terminate when $\text{gap} = 0$.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

void swapIfGreater(vector<int>& arr1, vector<int>& arr2, int ind1, int ind2) {
    if (arr1[ind1] > arr2[ind2]) {
        swap(arr1[ind1], arr2[ind2]);
    }
}

void merge(vector<int>& arr1, vector<int>& arr2) {
    int n = arr1.size();
    int m = arr2.size();
    int len = n + m;
    
    // Initial mathematical gap
    int gap = (len / 2) + (len % 2); 
    
    while (gap > 0) {
        int left = 0;
        int right = left + gap;
        
        while (right < len) {
            // Case 1: Left is in arr1, Right is in arr2
            if (left < n && right >= n) {
                swapIfGreater(arr1, arr2, left, right - n);
            } 
            // Case 2: Both are strictly in arr2
            else if (left >= n) {
                swapIfGreater(arr2, arr2, left - n, right - n);
            } 
            // Case 3: Both are strictly in arr1
            else {
                swapIfGreater(arr1, arr1, left, right);
            }
            left++;
            right++;
        }
        
        // Mathematical termination
        if (gap == 1) break;
        gap = (gap / 2) + (gap % 2);
    }
}
```

> [!important]
> The exact calculation of the ceiling `(gap / 2) + (gap % 2)` is an absolute mathematical requirement. Using standard integer division `gap / 2` will cause the gap to prematurely collapse to `0` from `1` before the final adjacent pass is completed, leaving the arrays unsorted.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}((N + M) \log(N + M))$. The gap shrinks logarithmically $\log_2(N+M)$ times. For each distinct gap, we perform a strictly linear scan of $N+M$ elements.
- **space Complexity:** $\mathcal{O}(1)$. The abstract index mapping completely eliminates the need for auxiliary storage.

NEXT: [[Index]]
