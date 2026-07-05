# Largest Rectangle in a Histogram

## Problem Statement
- find the area of the largest rectangle that can be formed within the bounds of a given histogram where each bar has width 1.

## Approach / Intuition
- the maximal rectangle constrained by a bar is determined by its [[Previous Smaller Element]] and [[Next Smaller Element]]. We can use a [[Monotonic Stack]] storing indices in increasing order of height. When we encounter a shorter bar, it marks the right boundary for the bars in the stack, allowing us to compute their rectangle areas.

## Time & Space Complexity
- **[[time Complexity]]:** O(N) where N is the number of bars, as each index is pushed and popped exactly once.
- **[[space Complexity]]:** O(N) for the stack.

## Sample Code
```cpp
long long getMaxArea(long long arr[], int n) {
    stack<int> st;
    long long max_area = 0;
    for (int i = 0; i <= n; i++) {
        while (!st.empty() && (i == n || arr[st.top()] >= arr[i])) {
            long long height = arr[st.top()];
            st.pop();
            long long width = st.empty() ? i : i - st.top() - 1;
            max_area = max(max_area, height * width);
        }
        st.push(i);
    }
    return max_area;
}
```

## New Keywords / STL Used
- `std::stack`, `std::max`

## Edge Cases
- all bars have the same height.
- histogram heights are strictly increasing or strictly decreasing.
- array containing a single element.

NEXT: [[Index]]
