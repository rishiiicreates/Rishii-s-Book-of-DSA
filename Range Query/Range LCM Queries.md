# Range LCM Queries

## Problem Statement
- answer queries to find the Least Common Multiple (LCM) of all elements in a range $[L, R]$.

## Approach / Intuition
- use a [[Segment Tree]] where each node stores the LCM of its segment. The LCM of two segments is computed as `(a * b) / GCD(a, b)`. Due to potential overflow, values can be stored under modulo if required by the problem.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N \log (\text{max\_val}))$ build, $O(\log N \log (\text{max\_val}))$ query
- **[[space Complexity]]:** $O(N)$

## Sample Code
```cpp
#include <vector>
#include <numeric>

using namespace std;

class LCMSegmentTree {
    vector<long long> tree;
    int n;

    long long lcm(long long a, long long b) {
        if (a == 0 || b == 0) return a + b;
        return (a / std::gcd(a, b)) * b;
    }

    void build(const vector<long long>& arr, int node, int start, int end) {
        if (start == end) {
            tree[node] = arr[start];
        } else {
            int mid = start + (end - start) / 2;
            build(arr, 2 * node, start, mid);
            build(arr, 2 * node + 1, mid + 1, end);
            tree[node] = lcm(tree[2 * node], tree[2 * node + 1]);
        }
    }

public:
    LCMSegmentTree(const vector<long long>& arr) {
        n = arr.size();
        tree.assign(4 * n, 1);
        build(arr, 1, 0, n - 1);
    }

    long long query(int node, int start, int end, int l, int r) {
        if (r < start || end < l) return 1;
        if (l <= start && end <= r) return tree[node];
        int mid = start + (end - start) / 2;
        long long p1 = query(2 * node, start, mid, l, r);
        long long p2 = query(2 * node + 1, mid + 1, end, l, r);
        if (p1 == 1) return p2;
        if (p2 == 1) return p1;
        return lcm(p1, p2);
    }
};
```

## New Keywords / STL Used
- `std::gcd` (C++17)

## Edge Cases
- large LCM causing integer overflow, queries on disjoint ranges.

NEXT: [[Index]]
