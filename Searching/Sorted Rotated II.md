---
type: concept
tags: [searching, binary-search, cpp, rotated-array]
date: 2026-06-30
---
# Search in Sorted Rotated Array II

## Problem Statement
Determine if a `target` value exists in a sorted array that has been rotated.
*Crucial Difference:* The array may contain **duplicate elements**. Return a boolean `true` if it exists, `false` otherwise.

---

## Approach: Indeterministic Boundary Shrinking

The presence of duplicates introduces a mathematical anomaly. If `nums[left] == nums[mid] == nums[right]`, it is mathematically impossible to deterministically identify which half of the array is strictly sorted. 
For example, in `[1, 0, 1, 1, 1]` and `[1, 1, 1, 0, 1]`, the `mid` is `1` for both, but the `0` is on the left in the first case and on the right in the second.

Algorithm:
1. Follow the standard deterministic half-validation logic of Rotated Array I.
2. If `nums[left] == nums[mid]` AND `nums[mid] == nums[right]`, we are in the indeterministic state.
3. To resolve this, we simply shrink the search space linearly by doing `left++` and `right--`. 
4. This systematically trims the duplicate edges until a deterministic monotonic boundary is revealed.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

bool searchRotatedII(const vector<int>& nums, int target) {
    if (nums.empty()) return false;
    
    int left = 0;
    int right = nums.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) return true;
        
        // The Indeterministic Anomaly: Cannot distinguish sorted half
        if (nums[left] == nums[mid] && nums[mid] == nums[right]) {
            left++;
            right--;
        } 
        // Standard Case 1: Left half is sorted
        else if (nums[left] <= nums[mid]) {
            if (target >= nums[left] && target < nums[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        } 
        // Standard Case 2: Right half is sorted
        else {
            if (target > nums[mid] && target <= nums[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    
    return false;
}
```

> [!warning]
> The indeterministic shrinking `left++` and `right--` breaks the $\mathcal{O}(\log N)$ guarantee. If the array is entirely composed of one number (e.g., `[1, 1, ..., 1]`), the algorithm is forced to degrade to a linear scan.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(\log N)$ on average for arrays with well-distributed data. In the absolute worst case (an array of identical elements where the target is not present), the complexity catastrophically degrades to $\mathcal{O}(N)$ due to continuous linear shrinking.
- **Space Complexity:** $\mathcal{O}(1)$. The algorithm requires strictly no auxiliary allocations.
