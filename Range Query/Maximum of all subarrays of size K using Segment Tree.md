---
type: concept
tags: [cpp, range_query, segment_tree]
date: 2026-06-30
---
# Maximum of all subarrays of size K using Segment Tree

## Problem Statement
Given an array and an integer $K$, find the maximum for each and every contiguous subarray of size $K$.

## Approach / Intuition
We can build a [[Segment Tree]] where each node stores the maximum of its range. Then, we query the maximum in the range $[i, i + K - 1]$ for all $i$ from $0$ to $N - K$. Alternatively, a Sliding Window approach is more optimal, but a Segment tree works in $O(N \log N)$.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N \log N)$
- **[[Space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

class MaxSegmentTree {
    vector<int> tree;
    int n;

    void build(const vector<int>& arr, int node, int start, int end) {
        if (start == end) {
            tree[node] = arr[start];
        } else {
            int mid = start + (end - start) / 2;
            build(arr, 2 * node, start, mid);
            build(arr, 2 * node + 1, mid + 1, end);
            tree[node] = max(tree[2 * node], tree[2 * node + 1]);
        }
    }

    int query(int node, int start, int end, int l, int r) {
        if (r < start || end < l) return -2147483648;
        if (l <= start && end <= r) return tree[node];
        int mid = start + (end - start) / 2;
        int p1 = query(2 * node, start, mid, l, r);
        int p2 = query(2 * node + 1, mid + 1, end, l, r);
        return max(p1, p2);
    }

public:
    MaxSegmentTree(const vector<int>& arr) {
        n = arr.size();
        tree.assign(4 * n, 0);
        if (n > 0) build(arr, 1, 0, n - 1);
    }
    
    int getMax(int l, int r) {
        return query(1, 0, n - 1, l, r);
    }
};

vector<int> maxSlidingWindow(vector<int>& arr, int k) {
    int n = arr.size();
    vector<int> res;
    if (n == 0 || k == 0) return res;
    MaxSegmentTree st(arr);
    for (int i = 0; i <= n - k; i++) {
        res.push_back(st.getMax(i, i + k - 1));
    }
    return res;
}
```

## New Keywords / STL Used
`std::max`

## Edge Cases
$K$ equals array size, $K = 1$.
