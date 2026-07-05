# Sort Colors (Dutch National Flag)

## Problem Statement
- given an array $A$ of $N$ objects containing discrete states $0$ (red), $1$ (white), and $2$ (blue), sort the objects in-place strictly in $O(N)$ time such that all identical colors are contiguous in the order $0, 1, 2$.


## Approach: 3-Way Partitioning (Dijkstra)

- a standard sorting algorithm evaluates to $O(N \log N)$. Counting sort evaluates to $O(N)$ but demands two absolute passes over the sequence.
- edsger W. Dijkstra formulated the **Dutch National Flag Algorithm** to execute the exact permutation in a strict single pass.

- we define three topological boundary pointers:
- $l$ (Low): Denotes the upper bound of the $0$ domain.
- $m$ (Mid): The exploratory cursor navigating the unpartitioned sequence.
- $h$ (High): Denotes the lower bound of the $2$ domain.

- the array is partitioned into four dynamic domains:
- $a[0 \dots L-1] = 0$
- $a[L \dots M-1] = 1$
- $a[M \dots H] = \text{Unexplored}$
- $a[H+1 \dots N-1] = 2$

- we iterate $M$ while $M \le H$:
- **if $A[M] == 0$:** Swap $A[L]$ and $A[M]$, increment both $L$ and $M$. (The element swapped into $M$ was previously behind $M$, thus it is strictly $1$, making it safe to increment $M$).
- **if $A[M] == 1$:** The element is structurally sound; increment $M$.
- **if $A[M] == 2$:** Swap $A[M]$ and $A[H]$, decrement $H$. (Do NOT increment $M$, because the element swapped from $H$ is unexplored and must be evaluated on the next cycle).


## Code Implementation

```cpp
#include <vector>
#include <utility>

using namespace std;

void sortColors(vector<int>& nums) {
    int low = 0;
    int mid = 0;
    int high = nums.size() - 1;
    
    // Evaluate unexplored sequence
    while (mid <= high) {
        if (nums[mid] == 0) {
            swap(nums[low], nums[mid]);
            low++;
            mid++;
        } else if (nums[mid] == 1) {
            mid++;
        } else { // nums[mid] == 2
            swap(nums[mid], nums[high]);
            high--;
        }
    }
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ strict single-pass linear time.
- **space Complexity:** $O(1)$ auxiliary memory.

> [!warning]
> **Mid Increment Asymmetry:** The crucial algorithmic trap lies in incrementing `mid`. When swapping with `low`, we can guarantee the incoming element is `1`, justifying `mid++`. When swapping with `high`, the incoming element is totally unknown, necessitating re-evaluation at the exact same `mid` index.

NEXT: [[Index]]
