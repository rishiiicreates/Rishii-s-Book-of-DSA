# Min-Max Range Queries

## Problem Statement
- given an array, efficiently answer queries to find the minimum and maximum elements in a given range $[L, R]$.

## Approach / Intuition
- we can build a [[Segment Tree]] where each node stores a pair of `{min, max}` for its corresponding segment. Merging two segments involves taking the minimum of their mins and the maximum of their maxes.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N)$ build, $O(\log N)$ query
- **[[space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

struct Node {
    int mn, mx;
};

class MinMaxSegmentTree {
    vector<Node> tree;
    int n;

    void build(const vector<int>& arr, int node, int start, int end) {
        if (start == end) {
            tree[node] = {arr[start], arr[start]};
        } else {
            int mid = start + (end - start) / 2;
            build(arr, 2 * node, start, mid);
            build(arr, 2 * node + 1, mid + 1, end);
            tree[node].mn = min(tree[2 * node].mn, tree[2 * node + 1].mn);
            tree[node].mx = max(tree[2 * node].mx, tree[2 * node + 1].mx);
        }
    }

public:
    MinMaxSegmentTree(const vector<int>& arr) {
        n = arr.size();
        tree.resize(4 * n);
        build(arr, 1, 0, n - 1);
    }

    Node query(int node, int start, int end, int l, int r) {
        if (r < start || end < l) return {2147483647, -2147483648};
        if (l <= start && end <= r) return tree[node];
        int mid = start + (end - start) / 2;
        Node left = query(2 * node, start, mid, l, r);
        Node right = query(2 * node + 1, mid + 1, end, l, r);
        return {min(left.mn, right.mn), max(left.mx, right.mx)};
    }
};
```

## New Keywords / STL Used
- `struct`, `std::min`, `std::max`

## Edge Cases
- range $[L, R]$ has only one element, query out of bounds.

NEXT: [[Index]]
