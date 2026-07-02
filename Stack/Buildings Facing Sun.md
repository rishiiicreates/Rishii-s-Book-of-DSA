---
type: concept
tags: [stack, cpp]
date: 2026-06-30
---
# Buildings Facing Sun

## Problem Statement
Given an array representing the heights of buildings, count the number of buildings that can see the sunrise. A building can see the sunrise if it is strictly taller than all buildings to its left.

## Approach / Intuition
Instead of a full [[Stack]], we can just track the maximum height seen so far. Iterating from left to right, if the current building's height is greater than the running maximum height, it can see the sun, and we update the maximum height.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) because we iterate through the array once.
- **[[Space Complexity]]:** O(1) as we only use a few variables for counting and tracking the maximum.

## Sample Code
```cpp
int countBuildings(int h[], int n) {
    int count = 1;
    int max_h = h[0];
    for(int i = 1; i < n; i++){
        if(h[i] > max_h){
            count++;
            max_h = h[i];
        }
    }
    return count;
}
```

## New Keywords / STL Used
- Simple loops and variables without complex STL.

## Edge Cases
- All buildings have the same height.
- Array contains only a single building.
- Buildings sorted in strictly decreasing order.
