---
type: concept
tags: [searching, cpp, linear-search]
date: 2026-06-30
---
# Largest Element

## Problem Statement
Given an unsorted array $A$ of size $N$, find the maximum element.

*Example:* $A = [3, 2, 1, 5, 2]$
*Result:* $5$

---

## Approach: Linear Search

Since the array is unsorted, we must inspect every single element at least once to ensure we don't miss the absolute maximum. We use a [[Linear Search]] approach.

1. Initialize a variable `maxVal` with the first element of the array (or negative infinity).
2. Iterate through each element in the array.
3. If the current element is strictly greater than `maxVal`, update `maxVal`.

---

## Code Implementation

```cpp
#include <vector>
using namespace std;

int findLargest(const vector<int>& nums) {
    if (nums.empty()) return -1; // Or throw an exception
    
    int maxVal = nums[0];
    for (size_t i = 1; i < nums.size(); i++) {
        if (nums[i] > maxVal) {
            maxVal = nums[i];
        }
    }
    
    return maxVal;
}
```

> [!tip]
> In C++ STL, you can simply use `*std::max_element(nums.begin(), nums.end())` from `<algorithm>` to achieve this in a single readable line.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ where $N$ is the number of elements. We traverse the array exactly once.
- **Space Complexity:** $O(1)$ auxiliary space. Only a single variable is maintained.
