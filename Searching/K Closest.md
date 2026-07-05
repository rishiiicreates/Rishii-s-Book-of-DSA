# Find K Closest Elements

## Problem Statement
- given a sorted integer array `arr`, two integers `k` and `x`, return the `k` closest integers to `x` in the array. The final result should be sorted in ascending order.
- integer $a$ is closer to $x$ than integer $b$ if:
- $|a - x| < |b - x|$, or
- $|a - x| == |b - x|$ and $a < b$


## Approach: Binary Searching the Window Boundary

- the naive approach is to sort the array by absolute difference, which takes $\mathcal{O}(N \log N)$.
- a better approach uses Binary Search to find the closest element to $x$ and expands two pointers outward, taking $\mathcal{O}(\log N + K)$.

- however, the **absolute optimal mathematical approach** is to directly binary search for the **starting index** of the optimal $K$-sized window!
- the starting index of the window must lie in the range $[0, N - K]$.
- for any candidate starting index `mid`, compare the distance of `x` to `arr[mid]` versus `arr[mid + k]`.
- if $x - arr[mid] > arr[mid + k] - x$, it implies that `arr[mid + k]` is mathematically closer to $x$ than `arr[mid]`. Thus, the optimal window must start further to the right. (`left = mid + 1`)
- otherwise, the window should shift leftwards. (`right = mid`)


## Code Implementation

```cpp
#include <vector>
#include <cmath>
using namespace std;

vector<int> findClosestElements(const vector<int>& arr, int k, int x) {
    // The valid boundary for the START of the window
    int left = 0;
    int right = arr.size() - k;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        // Compare distances at the exact boundaries of the virtual window
        if (x - arr[mid] > arr[mid + k] - x) {
            left = mid + 1; // Window needs to shift right
        } else {
            right = mid;    // Window needs to shift left (or stay)
        }
    }
    
    // Extract the K elements starting from the optimally found 'left' index
    return vector<int>(arr.begin() + left, arr.begin() + left + k);
}
```

> [!important]
> The condition `x - arr[mid] > arr[mid + k] - x` mathematically simplifies the absolute difference constraints because the array is strictly sorted. Thus, $arr[mid + k]$ is inherently $\ge arr[mid]$.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(\log(N - K) + K)$. The binary search converges on a search space of $N - K$ possible starting indices. Constructing the final vector slice takes $\mathcal{O}(K)$.
- **space Complexity:** $\mathcal{O}(K)$ strictly for allocating the output array. Auxiliary space for the search itself is $\mathcal{O}(1)$.

NEXT: [[Index]]
