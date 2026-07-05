# Minimum Size Subarray Sum

## Problem Statement
- given an array of positive integers and a target sum, find the minimal length of a contiguous subarray whose sum is greater than or equal to the target.

## Approach / Intuition
- use a [[Sliding Window]], which is a variation of the two-pointer technique. Maintain a window `[left, right]`. Expand the window by moving `right` and adding elements to the current sum. Once the sum meets the condition, try shrinking the window by moving `left` to find the minimal length.

## Time & Space Complexity
- **[[time Complexity]]:** O(n)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
int minSubArrayLen(int target, const vector<int>& nums) {
    int n = nums.size();
    int minLen = INT_MAX;
    int currentSum = 0;
    int left = 0;
    for (int right = 0; right < n; ++right) {
        currentSum += nums[right];
        while (currentSum >= target) {
            minLen = min(minLen, right - left + 1);
            currentSum -= nums[left];
            left++;
        }
    }
    return minLen == INT_MAX ? 0 : minLen;
}
```

## New Keywords / STL Used
- `iNT_MAX`, `std::min`

## Edge Cases
- no subarray meets the condition, all elements are larger than the target.

NEXT: [[Index]]
