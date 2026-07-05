# Max of Mins of every window size

## Problem Statement
- given an integer array, find the maximum of the minimums of every possible window size from 1 to N.

## Approach / Intuition
- for each element, we find the range over which it acts as the minimum using the [[Next Smaller Element]] and [[Previous Smaller Element]] via a [[Stack]]. We record the element as a candidate for the answer of that particular window length. Since larger elements can also be the answer for smaller windows, we backfill the answer array from right to left.

## Time & Space Complexity
- **[[time Complexity]]:** O(N) to compute left and right limits and populate the answers array.
- **[[space Complexity]]:** O(N) to maintain the stack and the bounds arrays.

## Sample Code
```cpp
vector<int> maxOfMin(int arr[], int n) {
    stack<int> s;
    vector<int> left(n + 1, -1), right(n + 1, n);
    for (int i = 0; i < n; i++) {
        while (!s.empty() && arr[s.top()] >= arr[i]) s.pop();
        if (!s.empty()) left[i] = s.top();
        s.push(i);
    }
    while (!s.empty()) s.pop();
    for (int i = n - 1; i >= 0; i--) {
        while (!s.empty() && arr[s.top()] >= arr[i]) s.pop();
        if (!s.empty()) right[i] = s.top();
        s.push(i);
    }
    vector<int> ans(n + 1, 0);
    for (int i = 0; i < n; i++) {
        int len = right[i] - left[i] - 1;
        ans[len] = max(ans[len], arr[i]);
    }
    for (int i = n - 1; i >= 1; i--) ans[i] = max(ans[i], ans[i + 1]);
    vector<int> res(n);
    for (int i = 1; i <= n; i++) res[i - 1] = ans[i];
    return res;
}
```

## New Keywords / STL Used
- `std::stack`, `std::vector`, `std::max`

## Edge Cases
- all elements are the same.
- elements are in strictly increasing or decreasing order.

NEXT: [[Index]]
