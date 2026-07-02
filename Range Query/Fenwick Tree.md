---
type: concept
tags: [dsa, cpp, range-query, tree]
date: 2026-06-30
---
# Fenwick Tree (Binary Indexed Tree)

## Problem Statement
Answer prefix sum queries and perform point updates on an array efficiently using minimal code and less memory than a Segment Tree.

## Approach / Intuition
A [[Fenwick Tree]] (BIT) stores cumulative sums in an array where each index `i` is responsible for a range of elements based on its least significant set bit (LSB). The LSB is computed using `i & (-i)`. Adding `LSB` navigates to the parent during point updates, while subtracting `LSB` navigates down to accumulate prefix sums.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N) to construct, O(log N) for update and query
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
class BIT {
    vector<int> tree;
    int n;
public:
    BIT(int size) {
        n = size;
        tree.assign(n + 1, 0);
    }
    void add(int i, int delta) {
        for (++i; i <= n; i += i & -i)
            tree[i] += delta;
    }
    int query(int i) {
        int sum = 0;
        for (++i; i > 0; i -= i & -i)
            sum += tree[i];
        return sum;
    }
};
```

## New Keywords / STL Used
Bitwise AND `&`, unary negation `-`

## Edge Cases
Zero indexing vs one indexing (BIT internally uses 1-based indexing).
