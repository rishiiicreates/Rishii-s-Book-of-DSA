---
type: concept
tags: [two-pointer, cpp, sum-problem]
date: 2026-06-30
---
# Pair Sum (Two Sum)

## Problem Statement
Given an **unsorted** integer array $A$ of size $N$ and an integer `target`, determine if there exists a pair of elements $A[i]$ and $A[j]$ (where $i \neq j$) such that $A[i] + A[j] = \text{target}$. 

*Example:* $A = [2, 7, 11, 15]$, $\text{target} = 9$
*Result:* `true` (Pairs are 2 and 7)

---

## Approach: Sort and Two Pointers

While the classical $O(N)$ approach utilizes a [[Hash Map]] to store expected complements, we can also solve this strictly using the [[Two-Pointer]] paradigm by manipulating the array's structure.

1. **Monotonicity:** The Two-Pointer convergence technique mathematically requires monotonicity. We must first sort the array $A$ in ascending order. This takes $O(N \log N)$ time.
2. **Convergence:** Initialize `left = 0` and `right = N - 1`.
3. Let $S = A[\text{left}] + A[\text{right}]$.
   - If $S == \text{target}$, we have found a valid pair. Return `true`.
   - If $S < \text{target}$, the sum is too small. Because the array is sorted, decrementing `right` would only make the sum even smaller. Therefore, we mathematically must increment `left` to increase the sum.
   - If $S > \text{target}$, the sum is too large. By symmetrical logic, we must decrement `right` to decrease the sum.
4. If `left` crosses or equals `right` without finding a match, no such pair exists.

---

## Code Implementation

```cpp
#include <vector>
#include <algorithm>
using namespace std;

bool hasPairSum(vector<int>& arr, int target) {
    // Step 1: Establish monotonicity
    sort(arr.begin(), arr.end());
    
    int left = 0;
    int right = arr.size() - 1;
    
    // Step 2: Converging two pointers
    while (left < right) {
        long long sum = (long long)arr[left] + arr[right];
        
        if (sum == target) {
            return true;
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    
    return false;
}
```

> [!warning]
> If the problem demands returning the original *indices* of the pair, you cannot simply sort the array, as sorting destroys the original index mapping. You must either use an array of pairs `std::pair<value, index>` or fall back to the $O(N)$ Hash Map approach.

---

## Complexity Analysis
- **Time Complexity:** $O(N \log N)$. Sorting dominates the execution time. The two-pointer traversal itself takes strictly $O(N)$ time.
- **Space Complexity:** $O(1)$ auxiliary space if an in-place sort is used (like Introsort in `std::sort`).
