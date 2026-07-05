# Sort Ternary Array (Dutch National Flag)

## Problem Statement
- given an array containing exactly three distinct types of elements (e.g., $0$s, $1$s, and $2$s), sort the array in strictly linear time $\mathcal{O}(N)$ and strictly constant auxiliary space $\mathcal{O}(1)$.


## Approach: Dutch National Flag Algorithm

- this is the classic Dijkstra's Dutch National Flag problem. A standard [[Sorting]] algorithm takes $\mathcal{O}(N \log N)$, and a counting sort requires two distinct passes over the array.
- we can solve it in a single pass using three pointers (`low`, `mid`, `high`) maintaining strict mathematical invariants:
- the sub-array $[0, \text{low}-1]$ strictly contains $0$s.
- the sub-array $[\text{low}, \text{mid}-1]$ strictly contains $1$s.
- the sub-array $[\text{mid}, \text{high}]$ is the unexplored region.
- the sub-array $[\text{high}+1, N-1]$ strictly contains $2$s.

- algorithm:
- we examine `arr[mid]`.
- if `arr[mid] == 0`: It belongs to the first partition. We swap `arr[low]` and `arr[mid]`, then strictly increment both `low` and `mid`. (We know `arr[low]` is at worst a $1$, which is valid to swap into `mid`).
- if `arr[mid] == 1`: It is already in the correct partition. We simply increment `mid`.
- if `arr[mid] == 2`: It belongs to the final partition. We swap `arr[mid]` and `arr[high]`, and strictly decrement `high`. We do **not** increment `mid` here, because the element swapped from `high` is currently unexplored and must be evaluated on the next iteration.
- the algorithm mathematically terminates when the unexplored region vanishes (`mid > high`).


## Code Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

void sortColors(vector<int>& nums) {
    int low = 0;
    int mid = 0;
    int high = nums.size() - 1;
    
    // The loop continues until the unexplored region [mid, high] vanishes
    while (mid <= high) {
        if (nums[mid] == 0) {
            swap(nums[low], nums[mid]);
            low++;
            mid++;
        } 
        else if (nums[mid] == 1) {
            mid++;
        } 
        else if (nums[mid] == 2) {
            swap(nums[mid], nums[high]);
            high--; // Do not increment mid, the swapped element is unevaluated
        }
    }
}
```

> [!important]
> The condition `mid <= high` is crucial. If it was strictly `mid < high`, the algorithm would leave the final element at index `high` unevaluated, structurally violating the final partition invariants.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. The distance between `mid` and `high` mathematically decreases by exactly $1$ on every iteration, leading to exactly $N$ iterations.
- **space Complexity:** $\mathcal{O}(1)$. Evaluated strictly in-place.

NEXT: [[Index]]
