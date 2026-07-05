# Remove Duplicates from Sorted Array

## Problem Statement
- given an integer array $A$ sorted in mathematically monotonic non-decreasing order, remove all scalar duplicates *in-place* such that each unique element appears exactly once. The relative topological order must be preserved. Return the exact scalar count of unique elements, $K$.


## Approach: Write Boundary Pointer

- because the sequence is pre-sorted, all mathematically identical scalar elements form contiguous topological blocks. This renders hash maps obsolete.
- we invoke a Two Pointer state machine:
- **fast Pointer ($R$):** Evaluates elements sequentially.
- **slow Pointer ($L$):** Represents the boundary of the strictly unique partitioned sequence.

- we initialize $L = 1$, as $A[0]$ is trivially the first unique element.
- the exploratory pointer $R$ iterates from $1$ to $N-1$:
- we structurally evaluate $A[R]$ against $A[L-1]$ (the last validated unique element).
- if $A[R] == A[L-1]$, it is a redundant scalar instance. We skip it.
- if $A[R] \ne A[L-1]$, a structural divergence occurred, meaning $A[R]$ is a completely new discrete value. We overwrite $A[L]$ with $A[R]$ and increment the boundary $L$.


## Code Implementation

```cpp
#include <vector>

using namespace std;

int removeDuplicates(vector<int>& nums) {
    if (nums.empty()) return 0;
    
    // Boundary of unique sequence
    int write_index = 1;
    
    for (int i = 1; i < nums.size(); i++) {
        // Divergence indicates a new distinct scalar
        if (nums[i] != nums[write_index - 1]) {
            nums[write_index] = nums[i];
            write_index++;
        }
    }
    
    return write_index;
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ strict linear traversal. The fast pointer visits every element exactly once.
- **space Complexity:** $O(1)$ auxiliary space.

> [!tip]
> **Two-Element Tolerance Variant:** A common derivative limits redundancy to at most $2$ occurrences. The algorithm scales gracefully. Instead of comparing $A[R]$ against $A[L-1]$, we compare $A[R]$ strictly against $A[L-2]$. If they diverge, the element is valid. This mathematical sliding-window constraint accommodates arbitrary constraints (at most $M$ occurrences) trivially by comparing against $A[L-M]$.

NEXT: [[Index]]
