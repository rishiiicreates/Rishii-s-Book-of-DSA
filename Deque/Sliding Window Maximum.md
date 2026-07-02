---
type: concept
tags: [deque, sliding-window, monotonic-queue, cpp]
date: 2026-06-30
---
# Sliding Window Maximum

## Problem Statement
Given an array $A$ of $N$ integers and an integer $K$, there is a sliding window of size $K$ which is moving from the very left of the array to the very right. You can only see the $K$ numbers in the window. Each time the sliding window moves right by one position, mathematically compute the maximum element in the window. 

*Example:* $A = [1, 3, -1, -3, 5, 3, 6, 7]$, $K = 3$
*Result:* $[3, 3, 5, 5, 6, 7]$

---

## Approach: Monotonic Decreasing Deque

A naive solution checks all $K$ elements in each of the $N - K + 1$ windows, yielding $O(N \cdot K)$ time complexity. This is suboptimal. We can reduce the time complexity strictly to $O(N)$ by employing a [[Monotonic Queue]] using a Double-Ended Queue ([[Deque]]). 

The core invariant we maintain is that the deque stores **indices** of array elements such that the values corresponding to these indices are strictly **monotonically decreasing** from front to back.

1. **Initialization:** Initialize an empty `std::deque<int> dq` to store indices, and a vector `res` to store the maximums.
2. **Iteration:** Loop through the array $A$ from index $i = 0$ to $N-1$.
3. **Out-of-Bounds Ejection:** Check if the element at the front of the deque has fallen completely out of the current window $[i - K + 1, i]$. If $dq.front() \le i - K$, we mathematically pop it from the front: `dq.pop_front()`.
4. **Monotonicity Maintenance:** Before inserting the current element $A[i]$, we must ensure the decreasing invariant. While the deque is not empty and the element at the back of the deque is $\le A[i]$, pop it from the back: `dq.pop_back()`.
   - *Proof of Correctness:* Any element in the current window that is smaller than $A[i]$ is mathematically useless. It cannot be the maximum of the current window, nor can it be the maximum of any future window because $A[i]$ is both larger and will stay in the window longer.
5. **Insertion:** Push the current index $i$ to the back of the deque: `dq.push_back(i)`.
6. **Result Harvesting:** Once the window has reached size $K$ (i.e., when $i \ge K - 1$), the index at the absolute front of the deque definitively points to the maximum element for this window. Append $A[dq.front()]$ to the results.

---

## Code Implementation

```cpp
#include <vector>
#include <deque>
using namespace std;

vector<int> maxSlidingWindow(const vector<int>& nums, int k) {
    vector<int> res;
    deque<int> dq;
    int n = nums.size();
    
    for (int i = 0; i < n; ++i) {
        // Step 1: Remove indices completely out of the current window
        if (!dq.empty() && dq.front() <= i - k) {
            dq.pop_front();
        }
        
        // Step 2: Enforce monotonically decreasing invariant
        // Elements strictly smaller than the current element are permanently obsoleted
        while (!dq.empty() && nums[dq.back()] <= nums[i]) {
            dq.pop_back();
        }
        
        // Step 3: Insert current index
        dq.push_back(i);
        
        // Step 4: Record the mathematical maximum once the window is fully formed
        if (i >= k - 1) {
            res.push_back(nums[dq.front()]);
        }
    }
    
    return res;
}
```

> [!important]
> Storing **indices** rather than absolute values in the deque is critical. Without indices, you mathematically lose the ability to accurately determine if an element at the front has expired and fallen out of the sliding window boundary.

---

## Complexity Analysis
- **Time Complexity:** $O(N)$. Although there is a `while` loop nested within the `for` loop, each of the $N$ indices is pushed onto the deque exactly once and popped from the deque at most once. The amortized aggregate operation count is strictly linear.
- **Space Complexity:** $O(K)$. The deque mathematically only stores indices corresponding to the current window of size $K$. Therefore, its maximal size is strictly bounded by $O(K)$.
