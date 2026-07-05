# Largest Rectangle in Histogram

## Problem Statement
- given an array of $N$ non-negative scalars representing the heights of spatial histogram bars (each bearing a uniform width of $1$), mathematically compute the area of the absolute largest geometrical rectangle strictly bounding within the histogram framework.


## Approach: Left/Right Monotonic Boundaries

- the area of any contiguous bounding rectangle dictated by a bottleneck height $H[i]$ geometrically depends on how far it can extend spatially to the left and right without encountering a height strictly smaller than $H[i]$.
- if $L[i]$ is the index of the **Previous Smaller Element** and $R[i]$ is the index of the **Next Smaller Element**, the absolute spatial width evaluates to:
$$ W[i] = R[i] - L[i] - 1 $$
- the corresponding bounded geometric area evaluates to:
$$ \text{Area}[i] = H[i] \times W[i] $$

- algorithm (Optimized Single-Pass):
- we can structurally fuse the evaluations by leveraging an expanding [[Monotonic Stack]].
- maintain a stack of strictly ascending spatial indices.
- if we encounter a height $H[i]$ strictly less than the height evaluated at `stack.top()`, it geometrically forces the `stack.top()` bar to terminate its rightward expansion. Thus, $R[\text{top}] = i$.
- we iteratively pop the `top`. The structural element *currently* residing beneath it on the stack represents its left boundary $L[\text{top}]$. If the stack is exhausted, $L[\text{top}] = -1$.
- algebraically evaluate the contained geometric area utilizing these resolved boundaries.


## Code Implementation

```cpp
#include <vector>
#include <stack>
#include <algorithm>

using namespace std;

long long largestRectangleArea(vector<int>& heights) {
    int n = heights.size();
    stack<int> s;
    long long max_area = 0;
    
    // Evaluate spatial sequence up to n (forceful final flush)
    for (int i = 0; i <= n; i++) {
        // Theoretical height of 0 acts as a universal smaller boundary to forcefully exhaust the stack
        int current_height = (i == n) ? 0 : heights[i];
        
        while (!s.empty() && current_height < heights[s.top()]) {
            int bottleneck_index = s.top();
            s.pop();
            
            long long bottleneck_height = heights[bottleneck_index];
            
            // Resolve algebraic boundaries
            int right_bound = i;
            int left_bound = s.empty() ? -1 : s.top();
            
            long long width = right_bound - left_bound - 1;
            max_area = max(max_area, bottleneck_height * width);
        }
        
        s.push(i);
    }
    
    return max_area;
}
```

> [!warning]
> The evaluation iteration explicitly ranges up to $i = N$. Pushing a theoretical synthetic scalar height of $0$ at index $N$ structurally guarantees that any monotonically ascending sequences remaining inside the stack are forcefully flushed and geometrically evaluated.


## Complexity Analysis
- **time Complexity:** $\mathcal{O}(N)$. Every index is pushed and popped exactly once, guaranteeing strict linearity.
- **space Complexity:** $\mathcal{O}(N)$. Bounded strictly by the LIFO dimension storing monotonic indices.

NEXT: [[Index]]
