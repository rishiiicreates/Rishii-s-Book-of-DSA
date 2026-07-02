---
type: concept
tags: [sorting, cpp, divide-and-conquer]
date: 2026-06-30
---
# Surpasser Count of Each Element

## Problem Statement
Given an array $A$ of $N$ integers, find the surpasser count for each element. The surpasser count of $A[i]$ is defined as the number of elements $A[j]$ such that $j > i$ and $A[j] > A[i]$.

*Example:* $A = [2, 7, 5, 3, 0, 8, 1]$
*Result:* $[4, 1, 1, 1, 2, 0, 0]$ (For $2$, the surpassers are $7, 5, 3, 8$)

---

## Approach: Divide and Conquer (Modified Merge Sort)

This is a structural variation of the Inversion Count problem. Instead of counting how many elements on the left are strictly greater than elements on the right, we want to know how many elements on the right are strictly greater than elements on the left.

We use a modified [[Merge Sort]] that sorts the array in **descending** order. Since elements move around during sorting, we must track their original indices using an array of `std::pair<value, original_index>`.

1. Divide the array into two halves.
2. During the merge step (sorting in descending order), we compare `left[i]` and `right[j]`.
3. If `left[i].value > right[j].value`, `left[i]` surpasses all elements remaining in the right half (since the right half is sorted descending). However, it's easier to count dynamically: whenever we pull an element from the right half, we increment a `right_pulled_count`. 
4. When we finally pull `left[i]` into the merged array, the number of elements from the right half that are strictly greater than `left[i]` is exactly `right_pulled_count`. We add this to the answer array at `left[i].original_index`.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

void mergeAndCount(vector<pair<int, int>>& arr, int left, int mid, int right, vector<int>& res) {
    vector<pair<int, int>> temp;
    temp.reserve(right - left + 1);
    
    int i = left;
    int j = mid + 1;
    int right_greater_count = 0;
    
    // Sort in DESCENDING order
    while (i <= mid && j <= right) {
        if (arr[i].first < arr[j].first) {
            // Element in right half is strictly greater
            right_greater_count++;
            temp.push_back(arr[j++]);
        } else {
            // Element in left half is pulled. It has been surpassed by `right_greater_count` elements.
            res[arr[i].second] += right_greater_count;
            temp.push_back(arr[i++]);
        }
    }
    
    // If elements remain in left half, they are smaller than all elements pulled from right half
    while (i <= mid) {
        res[arr[i].second] += right_greater_count;
        temp.push_back(arr[i++]);
    }
    
    while (j <= right) {
        temp.push_back(arr[j++]);
    }
    
    for (int k = 0; k < temp.size(); ++k) {
        arr[left + k] = temp[k];
    }
}

void mergeSortAndCount(vector<pair<int, int>>& arr, int left, int right, vector<int>& res) {
    if (left >= right) return;
    int mid = left + (right - left) / 2;
    
    mergeSortAndCount(arr, left, mid, res);
    mergeSortAndCount(arr, mid + 1, right, res);
    mergeAndCount(arr, left, mid, right, res);
}

vector<int> findSurpasserCount(const vector<int>& arr) {
    int n = arr.size();
    vector<pair<int, int>> indexedArr(n);
    for (int i = 0; i < n; ++i) {
        indexedArr[i] = {arr[i], i};
    }
    
    vector<int> res(n, 0);
    mergeSortAndCount(indexedArr, 0, n - 1, res);
    return res;
}
```

> [!tip]
> A structurally equivalent approach is sorting in *ascending* order, but tracking how many elements are pulled from the left half when moving an element from the right half.

---

## Complexity Analysis
- **Time Complexity:** $O(N \log N)$. Governed by the Merge Sort recursion tree.
- **Space Complexity:** $O(N)$. Required for the auxiliary `indexedArr` and temporary merge vectors.
