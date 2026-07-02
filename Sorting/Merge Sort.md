---
type: concept
tags: [sorting, cpp, divide-and-conquer]
date: 2026-06-30
---
# Merge Sort

## Problem Statement
Sort an array of $N$ integers in strictly ascending order using the Merge Sort algorithm.

---

## Approach: Divide and Conquer Recursion

[[Merge Sort]] is a deterministic, stable sorting algorithm based on the [[Divide and Conquer]] mathematical paradigm.
1. **Divide:** Recursively bisect the continuous array $[L, R]$ into two independent halves $[L, M]$ and $[M+1, R]$ until every sub-array has a size of $1$. (A sub-array of size $1$ is mathematically inherently sorted).
2. **Conquer / Merge:** Recursively merge two sorted sub-arrays into a single strictly sorted macro-array using a deterministic two-pointer approach, maintaining $\mathcal{O}(K)$ time for merging $K$ elements.

The merge operation uses an auxiliary array to hold the sorted sequence temporarily before writing it back to the original array.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

void merge(vector<int>& arr, int left, int mid, int right) {
    vector<int> temp;
    temp.reserve(right - left + 1); // Mathematical optimization to prevent reallocation
    
    int i = left;
    int j = mid + 1;
    
    // Two-pointer linear merge
    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) {
            temp.push_back(arr[i++]);
        } else {
            temp.push_back(arr[j++]);
        }
    }
    
    // Flush remaining elements
    while (i <= mid) temp.push_back(arr[i++]);
    while (j <= right) temp.push_back(arr[j++]);
    
    // Write back the merged, sorted segment
    for (int k = 0; k < temp.size(); ++k) {
        arr[left + k] = temp[k];
    }
}

void mergeSort(vector<int>& arr, int left, int right) {
    // Base Case: Inherently sorted
    if (left >= right) return;
    
    int mid = left + (right - left) / 2;
    
    // Divide
    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);
    
    // Conquer
    merge(arr, left, mid, right);
}
```

> [!tip]
> Notice the strict `<=` comparison in `arr[i] <= arr[j]`. This mathematical equality guarantees the **stability** of the sort; if two elements are equal, the left one is forcefully preferred, preserving their relative temporal order.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N \log N)$. The recursive tree has a strictly bounded height of $\log_2(N)$. At each level of the tree, the merge operations collectively process exactly $N$ elements.
- **Space Complexity:** $\mathcal{O}(N)$. The `temp` array systematically requires $\mathcal{O}(N)$ auxiliary space. Additionally, the recursion stack depth is bounded by $\mathcal{O}(\log N)$.
