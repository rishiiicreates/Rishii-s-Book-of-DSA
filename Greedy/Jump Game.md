# Jump Game

## Problem Statement
- given an array of non-negative integers where each element represents your maximum jump length at that position, determine if you can reach the last index.

## Approach / Intuition
- maintain a variable that represents the furthest index you can reach so far. Iterate through the array; if the current index is reachable, update the furthest reachable index. If the furthest index is greater than or equal to the last index, return true. This is an application of a [[Greedy Algorithm]].

## Time & Space Complexity
- **[[time Complexity]]:** O(N)
- **[[space Complexity]]:** O(1)

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

bool canJump(vector<int>& nums) {
    int maxReach = 0;
    for (int i = 0; i < nums.size(); i++) {
        if (i > maxReach) return false;
        maxReach = max(maxReach, i + nums[i]);
        if (maxReach >= nums.size() - 1) return true;
    }
    return true;
}
```

## New Keywords / STL Used
- `max`

## Edge Cases
- array with a single element
- zero at the beginning of the array

NEXT: [[Index]]
