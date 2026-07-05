# Find Original Array from Prefix Sum

## Problem Statement
- given a prefix sum array, reconstruct and return the original array.

## Approach / Intuition
- since `prefix[i] = prefix[i-1] + arr[i]`, we can mathematically invert this logic. The first element naturally remains identical, and every subsequent element `arr[i]` can be extracted by performing `prefix[i] - prefix[i-1]`. This serves as the foundational reverse operation of a standard [[Prefix Sum]].

## Time & Space Complexity
- **[[time Complexity]]:** O(N)
- **[[space Complexity]]:** O(N)

## Sample Code
```cpp
vector<int> findOriginalArray(vector<int>& prefix) {
    int n = prefix.size();
    if (n == 0) return {};
    vector<int> arr(n);
    arr[0] = prefix[0];
    for (int i = 1; i < n; i++) {
        arr[i] = prefix[i] - prefix[i-1];
    }
    return arr;
}
```

## New Keywords / STL Used
- none

## Edge Cases
- empty prefix array, prefix array of size 1, arrays carrying negative aggregated totals.

NEXT: [[Index]]
