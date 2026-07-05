# Remove Duplicates from Sorted Array

## Problem Statement
- given a sorted array, remove the duplicates in-place such that each element appears only once and return the new length.

## Approach / Intuition
- use a slow pointer that indicates the position of the last unique element found, and a fast pointer to iterate through the array. When the fast pointer encounters a new distinct element, advance the slow pointer and overwrite its value. This is a standard fast-slow [[Two Pointers]] pattern.

## Time & Space Complexity
- **[[time Complexity]]:** O(n)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
int removeDuplicates(vector<int>& nums) {
    if (nums.empty()) return 0;
    int slow = 0;
    for (int fast = 1; fast < nums.size(); ++fast) {
        if (nums[fast] != nums[slow]) {
            slow++;
            nums[slow] = nums[fast];
        }
    }
    return slow + 1;
}
```

## New Keywords / STL Used
- none specific

## Edge Cases
- empty array, array with all identical elements, array with all unique elements.

NEXT: [[Index]]
