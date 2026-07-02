---
type: concept
tags: [stack, cpp]
date: 2026-06-30
---
# Subarray Range Sum

## Problem Statement
Find the sum of the ranges of all contiguous subarrays. The range of a subarray is defined as the difference between its maximum and minimum elements.

## Approach / Intuition
The sum of all subarray ranges equals the sum of maximums of all subarrays minus the sum of minimums of all subarrays. We can use a [[Monotonic Stack]] to efficiently find the contribution of each element as a maximum and as a minimum by finding the [[Next Greater Element]]/[[Next Smaller Element]] and their previous counterparts.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) since elements are pushed and popped from the stack at most once in both passes.
- **[[Space Complexity]]:** O(N) for the stack.

## Sample Code
```cpp
long long subArrayRanges(vector<int>& nums) {
    long long res = 0;
    int n = nums.size();
    stack<int> st;
    for (int i = 0; i <= n; i++) {
        while (!st.empty() && (i == n || nums[st.top()] > nums[i])) {
            int j = st.top(); st.pop();
            int k = st.empty() ? -1 : st.top();
            res -= (long long)nums[j] * (i - j) * (j - k);
        }
        st.push(i);
    }
    while (!st.empty()) st.pop();
    for (int i = 0; i <= n; i++) {
        while (!st.empty() && (i == n || nums[st.top()] < nums[i])) {
            int j = st.top(); st.pop();
            int k = st.empty() ? -1 : st.top();
            res += (long long)nums[j] * (i - j) * (j - k);
        }
        st.push(i);
    }
    return res;
}
```

## New Keywords / STL Used
- `std::stack`, `std::vector`

## Edge Cases
- Empty array or an array with a single element (sum is 0).
- Subarrays with duplicate maximum or minimum elements (carefully handled by strict/non-strict inequalities to avoid double counting).
