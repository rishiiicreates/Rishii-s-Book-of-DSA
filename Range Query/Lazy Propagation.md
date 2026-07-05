# Lazy Propagation

## Problem Statement
- perform range updates and range queries efficiently on an array.

## Approach / Intuition
- when updating a range in a [[Segment Tree]], updating every element can take $O(N)$ time. With [[Lazy Propagation]], we delay the updates to children nodes until they are strictly required. This ensures both range updates and queries run in logarithmic time.

## Time & Space Complexity
- **[[time Complexity]]:** $O(\log N)$ for both range update and query
- **[[space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <vector>

using namespace std;

class LazySegmentTree {
    vector<long long> tree, lazy;
    int n;

public:
    LazySegmentTree(int size) {
        n = size;
        tree.assign(4 * n, 0);
        lazy.assign(4 * n, 0);
    }

    void updateRange(int node, int start, int end, int l, int r, long long val) {
        if (lazy[node] != 0) {
            tree[node] += (end - start + 1) * lazy[node];
            if (start != end) {
                lazy[2 * node] += lazy[node];
                lazy[2 * node + 1] += lazy[node];
            }
            lazy[node] = 0;
        }

        if (start > end || start > r || end < l) return;

        if (start >= l && end <= r) {
            tree[node] += (end - start + 1) * val;
            if (start != end) {
                lazy[2 * node] += val;
                lazy[2 * node + 1] += val;
            }
            return;
        }

        int mid = start + (end - start) / 2;
        updateRange(2 * node, start, mid, l, r, val);
        updateRange(2 * node + 1, mid + 1, end, l, r, val);
        tree[node] = tree[2 * node] + tree[2 * node + 1];
    }

    long long queryRange(int node, int start, int end, int l, int r) {
        if (lazy[node] != 0) {
            tree[node] += (end - start + 1) * lazy[node];
            if (start != end) {
                lazy[2 * node] += lazy[node];
                lazy[2 * node + 1] += lazy[node];
            }
            lazy[node] = 0;
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
- `std::vector`

## Edge Cases
- overlapping range updates, queries on partially updated segments.

NEXT: [[Index]]
