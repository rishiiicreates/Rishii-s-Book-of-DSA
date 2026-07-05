# Largest Element

## Problem Statement
- given an unsorted array $A$ of size $N$, find the maximum element.

- *example:* $A = [3, 2, 1, 5, 2]$
- *result:* $5$


## Approach: Linear Search

- since the array is unsorted, we must inspect every single element at least once to ensure we don't miss the absolute maximum. We use a [[Linear Search]] approach.

- initialize a variable `maxVal` with the first element of the array (or negative infinity).
- iterate through each element in the array.
- if the current element is strictly greater than `maxVal`, update `maxVal`.


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


## Complexity Analysis
- **time Complexity:** $O(N)$ where $N$ is the number of elements. We traverse the array exactly once.
- **space Complexity:** $O(1)$ auxiliary space. Only a single variable is maintained.

NEXT: [[Index]]
