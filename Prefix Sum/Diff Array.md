# Difference Array

## Problem Statement
- given an array initialized to 0, process multiple queries of the form `(L, R, X)` to add value `X` to all elements from index `L` to `R`. After processing all queries, output the updated array.

## Approach / Intuition
- utilize a structural [[Difference Array]] initialized with zeros. For a query bounded by `(L, R, X)`, increment `diff[L]` by `X` and decrement `diff[R+1]` by `X`. Once all queries execute optimally in O(1) time, sweeping a continuous [[Prefix Sum]] across the difference array perfectly reconstitutes the final state.

## Time & Space Complexity
- **[[time Complexity]]:** O(N + Q)
- **[[space Complexity]]:** O(N)

## Sample Code
```cpp
vector<int> processQueries(int n, vector<vector<int>>& queries) {
    vector<int> diff(n + 1, 0);
    for (auto& q : queries) {
        int L = q[0], R = q[1], X = q[2];
        diff[L] += X;
        if (R + 1 < n) {
            diff[R + 1] -= X;
        }
    }
    vector<int> res(n);
    res[0] = diff[0];
    for (int i = 1; i < n; i++) {
        res[i] = res[i - 1] + diff[i];
    }
    return res;
}
```

## New Keywords / STL Used
- none

## Edge Cases
- enormous query clusters, overlapping heavy intervals, interval updates aggressively hitting the furthest array boundaries.

NEXT: [[Index]]
