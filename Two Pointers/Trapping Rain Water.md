# Trapping Rain Water

## Problem Statement
- given an array sequence $H$ representing an elevation map where the width of each bar is $1$, mathematically compute the total topological volume of water it can trap after raining.
- let $N$ be the length of $H$. We must compute $\sum_{i=0}^{N-1} W_i$ where $W_i$ is the water trapped at index $i$.


## Approach: Two Pointers (Bounding Box Isomorphism)

- the volume of water trapped above index $i$ is strictly governed by the bounding maxima on both sides:
$$ W_i = \max(0, \min(\max_{0 \le j \le i} H_j, \max_{i \le k < N} H_k) - H_i) $$
- evaluating this naively takes $O(N^2)$ time. Precomputing prefix and suffix maxima reduces this to $O(N)$ time and $O(N)$ space.

- we can achieve optimal $O(1)$ auxiliary space using **Two Pointers** bounding the sequence.
- let $L$ and $R$ be pointers initialized at $0$ and $N-1$ respectively.
- maintain two state variables, $\text{max}_L$ and $\text{max}_R$.
- because the water trapped is bottlenecked by the *minimum* of the two boundaries, if $\text{max}_L < \text{max}_R$, the water at $L$ is strictly bounded by $\text{max}_L$ regardless of any interior heights!
- thus, we can safely compute $W_L = \text{max}_L - H_L$, increment $L$, and update $\text{max}_L$.
- conversely, if $\text{max}_R \le \text{max}_L$, the water at $R$ is strictly bounded by $\text{max}_R$, allowing us to evaluate $R$ independently.


## Code Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

int trap(const vector<int>& height) {
    if (height.empty()) return 0;
    
    int left = 0, right = height.size() - 1;
    int max_left = 0, max_right = 0;
    int total_water = 0;
    
    // Evaluate via bottleneck traversal
    while (left <= right) {
        if (height[left] <= height[right]) {
            if (height[left] >= max_left) {
                max_left = height[left];
            } else {
                total_water += max_left - height[left];
            }
            left++;
        } else {
            if (height[right] >= max_right) {
                max_right = height[right];
            } else {
                total_water += max_right - height[right];
            }
            right--;
        }
    }
    
    return total_water;
}
```


## Complexity Analysis
- **time Complexity:** $O(N)$ linear strict scan. Both pointers traverse exactly $N$ elements combined.
- **space Complexity:** $O(1)$ auxiliary space.

> [!important]
> **Monotonic Constraint:** The correctness of this Two Pointer approach hinges entirely on the monotonically non-decreasing nature of $\text{max}_L$ and $\text{max}_R$. It leverages the transitive inequality: if $\text{max}_L \le \text{max}_R$ and the true right peak is potentially even larger than $\text{max}_R$, the minimum constraint $\min(\text{max}_L, \text{true\_right\_peak})$ is *still* exactly equal to $\text{max}_L$.

NEXT: [[Index]]
