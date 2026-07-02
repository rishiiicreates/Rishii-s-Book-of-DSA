---
type: concept
tags: [array, cpp, permutation, math]
date: 2026-07-01
---
# Next Permutation

## Problem Statement
Implement the mathematical operation `next_permutation`, which rearranges an array of numbers into the lexicographically next greater permutation of numbers. If such an arrangement is not possible (the array is sorted in descending order), rearrange it as the lowest possible order (sorted in ascending order).
*Example:* `[1, 2, 3]` $\rightarrow$ `[1, 3, 2]`. `[3, 2, 1]` $\rightarrow$ `[1, 2, 3]`.

---

## Approach: Lexicographical Pivot Search

To find the lexicographically next permutation, we need to alter the sequence as far to the right as possible to minimize the magnitude of the change.

1. **Find the Pivot:**
   Traverse from right to left to find the first element that is strictly smaller than the element immediately to its right. Let this index be $i$. Mathematically, $A[i] < A[i+1]$.
   This means the subarray $A[i+1 \dots N-1]$ is monotonically decreasing and is currently at its maximum possible lexicographical permutation.

2. **Handle Edge Case (Last Permutation):**
   If no such $i$ exists ($i = -1$), the entire array is monotonically decreasing. This is the absolute largest permutation. The next permutation wraps around to the lowest order by simply reversing the entire array.

3. **Find the Successor:**
   If $i$ exists, we must swap $A[i]$ with the smallest element strictly greater than $A[i]$ from the right side. Since $A[i+1 \dots N-1]$ is decreasing, we traverse from right to left to find the first element $A[j] > A[i]$.

4. **Swap and Minimize:**
   Swap $A[i]$ and $A[j]$. Now the prefix is correctly incremented to the next lexicographical state. 
   However, the suffix $A[i+1 \dots N-1]$ is still in a descending order (its maximum state). To minimize the overall permutation, we must reset this suffix to its lowest order by reversing $A[i+1 \dots N-1]$. Since it is descending, a reversal perfectly sorts it in ascending order in $O(N)$ time.

---

## Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

void nextPermutation(vector<int>& nums) {
    int n = nums.size();
    
    // Step 1: Find the pivot (first decreasing element from the right)
    int i = n - 2;
    while (i >= 0 && nums[i + 1] <= nums[i]) {
        i--;
    }
    
    if (i >= 0) {
        // Step 2: Find the successor (first element > pivot from the right)
        int j = n - 1;
        while (nums[j] <= nums[i]) {
            j--;
        }
        // Step 3: Swap pivot and successor
        swap(nums[i], nums[j]);
    }
    
    // Step 4: Reverse the suffix to make it the lowest lexicographical order
    reverse(nums.begin() + i + 1, nums.end());
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. Finding the pivot takes $O(N)$, finding the successor takes $O(N)$, and reversing the suffix takes $O(N)$. Total time is bounded by $O(N)$.
- **Space Complexity:** $O(1)$ auxiliary space, since swapping and reversing are done strictly in-place.

> [!important]
> **STL Alternative:** 
> In competitive programming, you can simply use the built-in C++ STL function: `std::next_permutation(nums.begin(), nums.end());`. This internally performs the exact algorithm described above. However, interviewers will explicitly ask you to implement it manually.
