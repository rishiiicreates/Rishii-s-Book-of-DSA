---
type: concept
tags: [heap, cpp, pairs, combinations]
date: 2026-06-30
---
# Find K Pairs with Smallest Sums

## Problem Statement
Given two thoroughly individually sorted distinct integer arrays, optimally dynamically find the precise `K` interconnected element pairs strictly natively possessing the mathematically smallest combinatorially combined sums.

## Approach / Intuition
Treat the conceptualized pairs structurally as an aggregated grid matrix natively. Begin strictly by seeding pairs explicitly formed solely by the first core element of `nums1` and directly every initial element of `nums2` inside an isolated Min Heap structurally. After correctly popping the baseline minimum, explicitly push the geometrically adjacent paired sequence generated strictly by advancing the localized array index of strictly `nums1` to aggressively explore the frontier utilizing a dynamic [[Priority Queue]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(K log K)
- **[[Space Complexity]]:** O(K)

## Sample Code
```cpp
vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
    vector<vector<int>> res;
    if (nums1.empty() || nums2.empty() || k == 0) return res;
    auto cmp = [&nums1, &nums2](pair<int, int> a, pair<int, int> b) {
        return nums1[a.first] + nums2[a.second] > nums1[b.first] + nums2[b.second];
    };
    priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> pq(cmp);
    for (int i = 0; i < nums1.size() && i < k; i++) {
        pq.push({i, 0});
    }
    while (k-- > 0 && !pq.empty()) {
        auto idx_pair = pq.top();
        pq.pop();
        res.push_back({nums1[idx_pair.first], nums2[idx_pair.second]});
        if (idx_pair.second + 1 < nums2.size()) {
            pq.push({idx_pair.first, idx_pair.second + 1});
        }
    }
    return res;
}
```

## New Keywords / STL Used
`decltype`, Lambda comparator syntax

## Edge Cases
Both intrinsic arrays inherently drastically scaling functionally smaller numerically than `K`, absolutely totally entirely empty array constraints natively terminating logically early.
