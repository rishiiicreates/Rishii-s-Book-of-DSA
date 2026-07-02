---
type: concept
tags: [sorting, cpp, std-sort, algorithm]
date: 2026-07-01
---
# Sorting

## Problem Statement
Given an array $A$ of $N$ integers, rearrange the elements such that for all $0 \le i < j < N$, $A[i] \le A[j]$. 
Mathematically, produce a monotonically non-decreasing permutation of $A$.

---

## Approach: Introsort (Hybrid Sorting Algorithm)

While theoretically important to know basic algorithms like Quick Sort $O(N \log N)$ or Merge Sort $O(N \log N)$, manual implementations in production are prone to worst-case regressions or heavy constant factors. 

Modern C++ provides `std::sort`, which strictly implements **Introsort**. Introsort is a mathematically rigorous hybrid algorithm:
1. It begins with **Quick Sort** for cache locality and speed.
2. It tracks the recursion depth. If the depth exceeds $2 \times \lfloor \log_2 N \rfloor$, Quick Sort is degenerating toward $O(N^2)$ time complexity.
3. Introsort instantly switches to **Heap Sort** to guarantee an absolute worst-case $O(N \log N)$ bound.
4. For extremely small partitions (typically $N < 16$), it defers to **Insertion Sort**, which has optimal instruction cache usage and zero recursive overhead for small scales.

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Wrapper for std::sort implementing Introsort
void sortArray(vector<int>& arr) {
    // Sorts the entire array in-place in strictly O(N log N) worst-case time
    sort(arr.begin(), arr.end());
}
```

---

## Complexity Analysis
- **Time Complexity:** strictly $O(N \log N)$ in the best, average, and worst case due to the Introsort algorithm.
- **Space Complexity:** $O(\log N)$ auxiliary space for the Quick Sort recursion stack.

> [!important]
> **Stability:** `std::sort` is **NOT** a stable sort. If two elements evaluate as mathematically equal (e.g., $A[i] == A[j]$), their relative original order is not guaranteed to be preserved. If relative order is a mathematical requirement of the algorithm, you must use `std::stable_sort`, which implements Merge Sort and guarantees stability at the cost of $O(N)$ extra memory.
