# Check if Array Elements are Consecutive

## Problem Statement
- given an unsorted array $A$ of $N$ integers, determine if the elements can be rearranged to form a strictly contiguous consecutive sequence.

- *example:* $A = [5, 2, 3, 1, 4]$
- *result:* `true` (Can be arranged to $[1, 2, 3, 4, 5]$)

- *example:* $A = [5, 2, 3, 1, 6]$
- *result:* `false`


## Approach: Mathematical Bounding & Hash Set

- for an array of $N$ elements to form a strictly consecutive mathematical sequence, two conditions must hold simultaneously:
- **uniqueness Constraint:** All $N$ elements must be distinct. If there are duplicates, it's impossible to form a sequence of length $N$.
- **range Constraint:** Let the maximum element be $Max$ and the minimum be $Min$. For the sequence to be contiguous without gaps, the mathematical span must exactly match the number of elements: $Max - Min = N - 1$.

- we can check both conditions in a single pass $O(N)$ using a [[Hash Set]] to verify uniqueness while continuously tracking the maximum and minimum values.


## Code Implementation

```cpp
#include <vector>
#include <unordered_set>
#include <algorithm>
using namespace std;

bool isConsecutiveSequence(const vector<int>& nums) {
    if (nums.empty()) return true;
    
    int min_val = nums[0];
    int max_val = nums[0];
    unordered_set<int> seen;
    
    for (int num : nums) {
        // Uniqueness Constraint: If we see a duplicate, it cannot be consecutive
        if (seen.find(num) != seen.end()) {
            return false;
        }
        seen.insert(num);
        
        // Track mathematical bounds
        min_val = min(min_val, num);
        max_val = max(max_val, num);
    }
    
    // Range Constraint Check
    return (max_val - min_val) == (nums.size() - 1);
}
```

> [!tip]
> Notice how this differs from "Longest Consecutive Sequence". Here, we evaluate the *entire* array globally as one monolithic sequence, allowing us to mathematically bypass sequence tracing entirely in favor of simple bounds checking.


## Complexity Analysis
- **time Complexity:** $O(N)$. We perform exactly one pass over the array, performing $O(1)$ set insertions and bound updates per element.
- **space Complexity:** $O(N)$ auxiliary space for the `unordered_set` to guarantee $O(1)$ duplicate checking.

NEXT: [[Index]]
