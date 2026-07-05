# Sliding Window Maximum

## Problem Statement
- given an array and an integer `k`, find the maximum element in every contiguous subarray (window) of size `k`.

## Approach / Intuition
- use a [[Deque]] to store the indices of array elements. Maintain elements in strictly decreasing order of their values within the deque. For every new element, remove indices from the back if their corresponding array values are smaller. Remove indices from the front if they fall outside the current window `[i - k + 1]`.

## Time & Space Complexity
- **[[time Complexity]]:** O(n)
- **[[space Complexity]]:** O(k)

## Sample Code
```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    deque<int> dq;
    vector<int> res;
    for (int i = 0; i < nums.size(); ++i) {
        if (!dq.empty() && dq.front() == i - k) {
            dq.pop_front();
        }
        while (!dq.empty() && nums[dq.back()] < nums[i]) {
            dq.pop_back();
        }
        dq.push_back(i);
        if (i >= k - 1) {
            res.push_back(nums[dq.front()]);
        }
    }
    return res;
}
```

## New Keywords / STL Used
- `std::deque`

## Edge Cases
- `k` equals 1, `k` equals array size, strictly increasing array, strictly decreasing array.

NEXT: [[Index]]
