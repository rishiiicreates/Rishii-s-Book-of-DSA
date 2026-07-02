---
type: concept
tags: [searching, cpp, linear-search]
date: 2026-06-30
---
# Second Largest Element

## Problem Statement
Given an unsorted array $A$ of size $N$, find the strictly second largest element. If no such element exists (e.g., all elements are identical), return $-1$.

*Example:* $A = [12, 35, 1, 10, 34, 1]$
*Result:* $34$

---

## Approach: Single Pass Linear Search (Optimal)

A naive approach would sort the array in $O(N \log N)$ and pick the second distinct element from the back. An optimized approach uses a single [[Linear Search]] pass.

1. Maintain two variables: `largest` and `secondLargest`, initialized to a very small sentinel value.
2. Iterate through the array.
3. If $A[i] > \text{largest}$, then the old `largest` becomes the new `secondLargest`, and $A[i]$ becomes the new `largest`.
4. If $A[i] < \text{largest}$ but $A[i] > \text{secondLargest}$, we exclusively update `secondLargest`. (This ensures strict inequality).

---

## Code Implementation

```cpp
#include <vector>
#include <climits>
using namespace std;

int findSecondLargest(const vector<int>& nums) {
    if (nums.size() < 2) return -1;
    
    int largest = INT_MIN;
    int secondLargest = INT_MIN;
    
    for (int num : nums) {
        if (num > largest) {
            secondLargest = largest;
            largest = num;
        } else if (num > secondLargest && num != largest) {
            // Strictly second largest
            secondLargest = num;
        }
    }
    
    return (secondLargest == INT_MIN) ? -1 : secondLargest;
}
```

> [!warning]
> The condition `num != largest` is absolutely critical for finding the *strictly* second largest element. If the array is $[10, 10]$, without this check, the algorithm might falsely assign `secondLargest = 10`.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ where $N$ is the number of elements. We do this in one single pass.
- **Space Complexity:** $O(1)$ auxiliary space.
