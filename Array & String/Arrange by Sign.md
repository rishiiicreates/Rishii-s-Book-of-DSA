---
type: concept
tags: [array, cpp, two-pointers]
date: 2026-06-30
---
# Arrange by Sign

## Problem Statement
Given an array of size $N$ with an equal number of positive and negative integers, rearrange the elements such that positive and negative numbers alternate. The relative order of positive and negative elements must be preserved. The first element of the rearranged array should be positive.

*Example:* $A = [3, 1, -2, -5, 2, -4]$
*Result:* $[3, -2, 1, -5, 2, -4]$

---

## Approach: Two Pointers with Auxiliary Array (Optimal)

Because we must preserve the relative order, an in-place swapping approach is generally $O(N^2)$ (unless we use complex cyclic shifts). The optimal way in time complexity is to use an auxiliary array of size $N$ and two pointers.

1. Initialize `posIndex = 0` for positive numbers and `negIndex = 1` for negative numbers.
2. Iterate through the array. 
3. If an element is positive, place it at `posIndex` and increment `posIndex` by $2$.
4. If an element is negative, place it at `negIndex` and increment `negIndex` by $2$.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

vector<int> arrangeBySign(const vector<int>& arr) {
    int n = arr.size();
    vector<int> res(n);
    
    int posIndex = 0;
    int negIndex = 1;
    
    for (int i = 0; i < n; ++i) {
        if (arr[i] > 0) {
            res[posIndex] = arr[i];
            posIndex += 2;
        } else {
            res[negIndex] = arr[i];
            negIndex += 2;
        }
    }
    
    return res;
}
```

> [!important]
> What if the number of positive and negative elements is *not* equal? The standard algorithm fails. You must first extract positives and negatives into two separate arrays, zip them up to the minimum count, and then append the remaining elements of the larger array.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. We iterate through the array of size $N$ exactly once.
- **Space Complexity:** $O(N)$. We require an auxiliary array of size $N$ to store the rearranged elements.
