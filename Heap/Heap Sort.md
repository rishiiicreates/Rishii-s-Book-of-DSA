---
type: concept
tags: [heap, dsa, cpp]
date: 2026-06-30
---
# Heap Sort

## Problem Statement
Sort an [[Array]] of elements in ascending or descending order using the [[Heap]] data structure.

## Approach / Intuition
First, build a max-heap from the given array (taking O(N) time). Then, repeatedly extract the maximum element (the root) and place it at the end of the array, while shrinking the considered heap size and calling heapify to restore the max-heap property. This results in a sorted array in-place.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log N)
- **[[Space Complexity]]:** O(1) auxiliary space, performed in-place.

## Sample Code
```cpp
#include <vector>

using namespace std;

void heapify(vector<int>& arr, int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    if (left < n && arr[left] > arr[largest])
        largest = left;

    if (right < n && arr[right] > arr[largest])
        largest = right;

    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

void heapSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }
    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}
```

## New Keywords / STL Used
`swap`

## Edge Cases
- Array already sorted or reverse sorted.
- Array with all identical elements.
