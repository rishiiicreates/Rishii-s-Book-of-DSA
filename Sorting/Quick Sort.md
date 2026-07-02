---
type: concept
tags: [sorting, cpp, divide-and-conquer, quick-sort]
date: 2026-06-30
---
# Quick Sort

## Problem Statement
Implement the standard Quick Sort algorithm to sort an array of integers in strictly ascending order.

---

## Approach: Divide-and-Conquer Partitioning

[[Quick Sort]] operates on the mathematical principle of recursive partitioning. 
1. **Pivot Selection:** Select an arbitrary element (e.g., the last element) as the `pivot`.
2. **Partitioning:** Rearrange the array such that all elements strictly less than the `pivot` are moved to its left, and all elements strictly greater than or equal to the `pivot` are moved to its right. The `pivot` is then mathematically guaranteed to be in its absolute final sorted position.
3. **Recursion:** Recursively apply this logic to the left sub-array and the right sub-array.

The algorithm relies on maintaining a boundary index `i` that tracks the rightmost boundary of the "less than pivot" region. As `j` scans the array, if a smaller element is found, it is swapped into the `i` boundary, expanding it.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    
    // Index of the smaller element boundary
    int i = low - 1; 
    
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    // Place the pivot in its mathematically correct sorted position
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(vector<int>& arr, int low, int high) {
    // Base Case: Sub-array of size 0 or 1 is inherently sorted
    if (low < high) {
        int pi = partition(arr, low, high);
        
        // Recursively sort the independent halves
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```

> [!warning]
> The basic Lomuto partition scheme (always picking `high` as pivot) degrades catastrophically to $\mathcal{O}(N^2)$ if the array is already sorted or reverse-sorted. For production use, a randomized pivot or Median-of-Three pivot selection is mathematically mandated to guarantee $\mathcal{O}(N \log N)$ expected time.

---

## Complexity Analysis
- **Time Complexity:** 
  - **Expected:** $\mathcal{O}(N \log N)$. The partitioning divides the array roughly in half.
  - **Worst Case:** $\mathcal{O}(N^2)$. Occurs when the partition strictly isolates 1 element (e.g., already sorted array).
- **Space Complexity:** $\mathcal{O}(\log N)$ expected auxiliary space for the recursion stack, degrading to $\mathcal{O}(N)$ in the worst case due to unbalanced recursion depth.
