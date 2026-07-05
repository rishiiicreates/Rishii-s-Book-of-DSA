# Count Strictly Increasing Subarrays

## Problem Statement
- given an array $A$ of $N$ integers, count the total number of strictly increasing contiguous subarrays.
- *example:* `[1, 2, 2, 4]` contains the strictly increasing subarrays `[1], [2], [2], [4], [1, 2], [2, 4]`. The total count is $6$.


## Approach: Combinatorics & Linear Scan

- any individual element is technically a strictly increasing subarray of length 1, but usually, problem statements imply counting subarrays of length $\ge 1$ (the sum logic remains the same).

- let's observe the mathematical property of subarrays:
- if we have a strictly increasing contiguous subarray of length $L$, the number of valid increasing subarrays ending exactly at the last element is exactly $L$.
- for example, if the array ends with a sequence of length 3 like `[1, 2, 3]`, the subarrays ending at `3` are `[3]`, `[2, 3]`, and `[1, 2, 3]` (count = 3).

- maintain `count = 0` and `current_len = 1` (since the first element itself is an increasing sequence of length 1).
- start iterating from index $i = 1$ to $N-1$:
   - if $A[i] > A[i-1]$, the element extends the strictly increasing sequence. We increment `current_len`.
   - if $A[i] \le A[i-1]$, the strict sequence is broken. We reset `current_len = 1`.
   - add `current_len - 1` to `count`. (Note: We subtract 1 because we often just want to count sequences of length $> 1$, depending on exactly how the problem is phrased. If the problem counts length 1 sequences as well, you would initialize `count = 1` and add `current_len` directly. The implementation below calculates sequences of length $> 1$, which is the standard competitive programming variation).


## Code Implementation

```cpp
#include <iostream>
#include <vector>

using namespace std;

long long countIncreasing(const vector<int>& arr) {
    if (arr.empty()) return 0;
    
    long long count = 0;
    long long current_len = 1;
    
    for (int i = 1; i < arr.size(); ++i) {
        if (arr[i] > arr[i - 1]) {
            // Sequence is strictly increasing
            current_len++;
        } else {
            // Sequence broken, reset length
            current_len = 1;
        }
        
        // Add all subarrays of length >= 2 ending at index i
        count += (current_len - 1);
    }
    
    return count;
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ where $N$ is the length of the array, requiring a single linear traversal.
- **space Complexity:** $O(1)$ auxiliary space since we just track `count` and `current_len`.

> [!note]
> **Subarrays of Length 1:** 
> The code provided calculates strictly increasing subarrays of length $\ge 2$. If the problem specifies that length 1 subarrays also count, simply change the `count` addition to `count += current_len` and start with an initial `count = 1` for the element at index 0. Always clarify the exact definition of "subarray" in these contexts!

NEXT: [[Index]]
