# Count Inversions

## Problem Statement
- given an array $A$ of $N$ integers, count the total number of inversions. Two elements $A[i]$ and $A[j]$ form an inversion if $i < j$ and $A[i] > A[j]$.

- *example:* $A = [2, 4, 1, 3, 5]$
- *result:* $3$ (Inversions are $(2,1), (4,1), (4,3)$)


## Approach: Divide and Conquer (Merge Sort)

- a brute-force approach requires checking every pair, taking $O(N^2)$ time. We can count inversions in $O(N \log N)$ time by augmenting the [[Merge Sort]] algorithm.

- **divide:** Split the array into two halves recursively until base cases (arrays of size 1) are reached. A single element has 0 inversions.
- **conquer:** The total inversions in a merged array is the sum of:
   - inversions completely inside the left half.
   - inversions completely inside the right half.
   - split inversions: where $A[i]$ is in the left half, $A[j]$ is in the right half, and $A[i] > A[j]$.
- **merge Step (Counting Split Inversions):** As we merge two sorted halves `left` and `right`, we maintain pointers `i` and `j`.
   - if `left[i] <= right[j]`, there is no inversion. We copy `left[i]` to the merged array and increment `i`.
   - if `left[i] > right[j]`, an inversion is found. Since the left half is sorted, *every* element from `i` to the end of the left half is also strictly greater than `right[j]`. We mathematically add `(mid - i + 1)` to our inversion count, copy `right[j]`, and increment `j`.


## Code Implementation

```cpp
#include <vector>
using namespace std;

long long mergeAndCount(vector<int>& arr, int left, int mid, int right) {
    vector<int> temp;
    temp.reserve(right - left + 1);
    
    int i = left;
    int j = mid + 1;
    long long inv_count = 0;
    
    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) {
            temp.push_back(arr[i++]);
        } else {
            temp.push_back(arr[j++]);
            // If arr[i] > arr[j], then all remaining elements in the left half
            // are also > arr[j] because the left half is sorted.
            inv_count += (mid - i + 1);
        }
    }
    
    // Copy remaining elements
    while (i <= mid) temp.push_back(arr[i++]);
    while (j <= right) temp.push_back(arr[j++]);
    
    // Write back to original array
    for (int k = 0; k < temp.size(); ++k) {
        arr[left + k] = temp[k];
    }
    
    return inv_count;
}

long long mergeSortAndCount(vector<int>& arr, int left, int right) {
    long long inv_count = 0;
    if (left < right) {
        int mid = left + (right - left) / 2;
        
        inv_count += mergeSortAndCount(arr, left, mid);
        inv_count += mergeSortAndCount(arr, mid + 1, right);
        inv_count += mergeAndCount(arr, left, mid, right);
    }
    return inv_count;
}
```

> [!important]
> Use `long long` for the inversion count. In the worst case (a strictly descending array), the number of inversions is $\frac{N(N-1)}{2}$, which will overflow a standard 32-bit signed integer if $N \ge 10^5$.


## Complexity Analysis
- **time Complexity:** $O(N \log N)$. This is fundamentally a Merge Sort operation.
- **space Complexity:** $O(N)$ for the temporary merge buffer.

NEXT: [[Index]]
