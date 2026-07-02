---
type: concept
tags: [searching, binary-search, cpp, rotated-array]
date: 2026-06-30
---
# Search in Sorted & Rotated Array

## Problem Statement
Given an array of integers `nums` sorted in ascending order (with distinct values) and rotated at some unknown pivot index $k$, find the index of a `target` value. If it is not found, return `-1`.

---

## Approach: Deterministic Half-Validation

A standard Binary Search requires strict monotonicity. A rotated sorted array is not strictly monotonic, but it consists of **two strictly monotonic segments**. 
The mathematical guarantee here is that if we pick any arbitrary `mid` point, at least one of the two halves—either `[left, mid]` or `[mid, right]`—**must** be strictly sorted.

Algorithm:
1. Find `mid`. If `nums[mid] == target`, return `mid`.
2. Check if the left half `[left, mid]` is sorted (i.e., `nums[left] <= nums[mid]`).
   - If sorted, check if `target` mathematically falls strictly within this bounded interval `[nums[left], nums[mid])`. 
     - If yes, eliminate the right half (`right = mid - 1`).
     - If no, eliminate the left half (`left = mid + 1`).
3. Otherwise, the right half `[mid, right]` must be strictly sorted.
   - Check if `target` mathematically falls strictly within this bounded interval `(nums[mid], nums[right]]`.
     - If yes, eliminate the left half (`left = mid + 1`).
     - If no, eliminate the right half (`right = mid - 1`).

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

int searchRotated(const vector<int>& nums, int target) {
    if (nums.empty()) return -1;
    
    int left = 0;
    int right = nums.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) return mid;
        
        // Case 1: Left half is strictly sorted
        if (nums[left] <= nums[mid]) {
            // Check if target is bounded within the sorted left half
            if (target >= nums[left] && target < nums[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        } 
        // Case 2: Right half must be strictly sorted
        else {
            // Check if target is bounded within the sorted right half
            if (target > nums[mid] && target <= nums[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    
    return -1;
}
```

> [!tip]
> Be extremely careful with the equality bounds `<` vs `<=`. Since we already check `nums[mid] == target` at the very beginning of the loop, the bounding conditions for `target < nums[mid]` and `target > nums[mid]` become strictly exclusive for the `mid` point.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(\log N)$. Despite the rotation, the search space is mathematically halved at every single step based on boundary evaluations.
- **Space Complexity:** $\mathcal{O}(1)$. The algorithm operates entirely in-place.
