# 3 Sum Closest

## Problem Statement
- given an array of integers and a target value, find three integers in the array such that their sum is closest to the given target. Return the sum of the three integers.

## Approach / Intuition
- sort the [[Array]]. Iterate through the array, fixing one element at a time. Use a [[Two-Pointer]] strategy for the remaining elements, similar to standard 3Sum. Track the absolute difference between the current sum and the target, updating the closest sum whenever a smaller difference is found. Move the pointers based on whether the sum is too small or too large.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N^2)$
- **[[space Complexity]]:** $O(1)$ auxiliary

## Sample Code
```cpp
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;

int threeSumClosest(vector<int>& nums, int target) {
    sort(nums.begin(), nums.end());
    int n = nums.size();
    int closestSum = nums[0] + nums[1] + nums[2];
    
    for (int i = 0; i < n - 2; i++) {
        int left = i + 1, right = n - 1;
        while (left < right) {
            int currentSum = nums[i] + nums[left] + nums[right];
            if (abs(currentSum - target) < abs(closestSum - target)) {
                closestSum = currentSum;
            }
            
            if (currentSum < target) {
                left++;
            } else if (currentSum > target) {
                right--;
            } else {
                return target;
            }
        }
    }
    return closestSum;
}
```

## New Keywords / STL Used
- `abs`, `sort`

## Edge Cases
- array size is exactly 3, multiple triplets with the same distance to target.

NEXT: [[Index]]
