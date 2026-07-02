---
type: concept
tags: [cpp, range_query]
date: 2026-06-30
---
# Flipping Sign

## Problem Statement
Given an array, perform updates that flip the sign of all elements in a range $[L, R]$, and query the sum of a range.

## Approach / Intuition
Using a [[Segment Tree]] with [[Lazy Propagation]], each node maintains the sum of its segment. Flipping the signs in a segment means multiplying the sum by $-1$. The lazy array stores whether a sign flip is pending for the children. 

## Time & Space Complexity
- **[[Time Complexity]]:** $O(\log N)$ for update and query
- **[[Space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <vector>

using namespace std;

class FlipSegmentTree {
    vector<long long> tree;
    vector<bool> lazy;
    int n;

public:
    FlipSegmentTree(const vector<long long>& arr) {
        n = arr.size();
        tree.assign(4 * n, 0);
        lazy.assign(4 * n, false);
        build(arr, 1, 0, n - 1);
    }

    void build(const vector<long long>& arr, int node, int start, int end) {
        if (start == end) {
            tree[node] = arr[start];
            return;
        }
        int mid = start + (end - start) / 2;
        build(arr, 2 * node, start, mid);
        build(arr, 2 * node + 1, mid + 1, end);
        tree[node] = tree[2 * node] + tree[2 * node + 1];
    }

    void updateRange(int node, int start, int end, int l, int r) {
        if (lazy[node]) {
            tree[node] = -tree[node];
            if (start != end) {
                lazy[2 * node] = !lazy[2 * node];
                lazy[2 * node + 1] = !lazy[2 * node + 1];
            }
            lazy[node] = false;
        }
        if (start > end || start > r || end < l) return;
        if (start >= l && end <= r) {
            tree[node] = -tree[node];
            if (start != end) {
                lazy[2 * node] = !lazy[2 * node];
                lazy[2 * node + 1] = !lazy[2 * node + 1];
            }
            return;
        }
        int mid = start + (end - start) / 2;
        updateRange(2 * node, start, mid, l, r);
        updateRange(2 * node + 1, mid + 1, end, l, r);
        tree[node] = tree[2 * node] + tree[2 * node + 1];
    }

    long long queryRange(int node, int start, int end, int l, int r) {
        if (lazy[node]) {
            tree[node] = -tree[node];
            if (start != end) {
                lazy[2 * node] = !lazy[2 * node];
                lazy[2 * node + 1] = !lazy[2 * node + 1];
            }
            lazy[node] = false;
        }
        if (start > end || start > r || end < l) return 0;
        if (start >= l && end <= r) return tree[node];
        int mid = start + (end - start) / 2;
        long long p1 = queryRange(2 * node, start, mid, l, r);
        long long p2 = queryRange(2 * node + 1, mid + 1, end, l, r);
        return p1 + p2;
    }
};
```

## New Keywords / STL Used
`std::vector`

## Edge Cases
Double flip restoring the original value, query after flip.
