---
type: concept
tags: [cpp, range_query]
date: 2026-06-30
---
# Segment Tree

## Problem Statement
Perform range queries and point updates on an array efficiently.

## Approach / Intuition
A [[Segment Tree]] divides the array into segments and stores aggregated information (like sum, min, max) for each segment. This allows range queries and point updates in logarithmic time.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$ build, $O(\log N)$ query/update
- **[[Space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <vector>

using namespace std;

class SegmentTree {
    vector<int> tree;
    int n;

    void build(const vector<int>& arr, int node, int start, int end) {
        if (start == end) {
            tree[node] = arr[start];
        } else {
            int mid = start + (end - start) / 2;
            build(arr, 2 * node, start, mid);
            build(arr, 2 * node + 1, mid + 1, end);
            tree[node] = tree[2 * node] + tree[2 * node + 1];
        }
    }

public:
    SegmentTree(const vector<int>& arr) {
        n = arr.size();
        tree.assign(4 * n, 0);
        build(arr, 1, 0, n - 1);
    }

    void update(int node, int start, int end, int idx, int val) {
        if (start == end) {
            tree[node] = val;
        } else {
            int mid = start + (end - start) / 2;
            if (start <= idx && idx <= mid) {
                update(2 * node, start, mid, idx, val);
            } else {
                update(2 * node + 1, mid + 1, end, idx, val);
            }
            tree[node] = tree[2 * node] + tree[2 * node + 1];
        }
    }

    int query(int node, int start, int end, int l, int r) {
        if (r < start || end < l) return 0;
        if (l <= start && end <= r) return tree[node];
        int mid = start + (end - start) / 2;
        int p1 = query(2 * node, start, mid, l, r);
        int p2 = query(2 * node + 1, mid + 1, end, l, r);
        return p1 + p2;
    }
};
```

## New Keywords / STL Used
`std::vector::assign`

## Edge Cases
Query range fully outside segment, fully inside, or partially overlapping.
