# Subarray with given sum

## Problem Statement
- given an array of non-negative integers, find a continuous subarray that adds up to a given target sum.

## Approach / Intuition
- use a [[Sliding Window]] approach. Add elements to a running sum by moving the right pointer. While the sum exceeds the target, subtract elements from the left. This works efficiently because all numbers are non-negative.

## Time & Space Complexity
- **[[time Complexity]]:** O(N) where N is the number of elements.
- **[[space Complexity]]:** O(1) auxiliary space.

## Sample Code
```cpp
vector<int> subarraySum(vector<int>& arr, int target) {
    int left = 0, currentSum = 0;
    for (int right = 0; right < arr.size(); ++right) {
        currentSum += arr[right];
        while (currentSum > target && left <= right) {
            currentSum -= arr[left];
            left++;
        }
        if (currentSum == target) {
            return {left, right};
        }
    }
    return {-1};
}
```

## New Keywords / STL Used
- none

## Edge Cases
- target is 0
- no subarray matches the target
- array contains zeros

NEXT: [[Index]]
