---
type: concept
tags: [queue, deque, cpp, array, two-pointers]
date: 2026-06-30
---
# Trapping Rain Water

## Problem Statement
Given `n` non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

## Approach / Intuition
While typically solved with [[Two Pointers]] or precomputed left/right arrays, it can also be approached with a monotonically decreasing [[Stack]] or Deque. The amount of water above a block depends on the minimum of the highest blocks to its left and right, minus its own height. Two pointers (`left` and `right`) tracking `leftMax` and `rightMax` provide the most optimal space complexity.

## Time & Space Complexity
- **[[Time Complexity]]:** O(n)
- **[[Space Complexity]]:** O(1) using two pointers

## Sample Code
```cpp
int trap(vector<int>& height) {
    int left = 0, right = height.size() - 1;
    int leftMax = 0, rightMax = 0;
    int water = 0;
    while (left < right) {
        if (height[left] <= height[right]) {
            if (height[left] >= leftMax) leftMax = height[left];
            else water += leftMax - height[left];
            left++;
        } else {
            if (height[right] >= rightMax) rightMax = height[right];
            else water += rightMax - height[right];
            right--;
        }
    }
    return water;
}
```

## New Keywords / STL Used
None specific

## Edge Cases
Empty array, strictly increasing/decreasing elevation, flat terrain.
