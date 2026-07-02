---
type: concept
tags: [dsa, cpp, trie, bit-manipulation, offline-queries]
date: 2026-06-30
---
# Maximum XOR With an Element From Array

## Problem Statement
Given an array and multiple queries of the form `[x, m]`, find the maximum XOR of `x` with any array element that is $\le m$.

## Approach / Intuition
Sort the array and the queries (offline query processing). The queries are sorted based on their $m$ values. Iterate through the sorted queries. Before evaluating a query, insert all array elements that are $\le m$ into a binary [[Trie]]. Once the valid elements are inserted, query the Trie to find the max XOR using the same [[Bit Manipulation]] technique as finding the max XOR of two numbers.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(Q \log Q + N \log N + Q \cdot 32 + N \cdot 32)$
- **[[Space Complexity]]:** $O(N \cdot 32 + Q)$ for Trie and offline query storage

## Sample Code
```cpp
#include <vector>
#include <algorithm>

using namespace std;

struct Node {
    Node* links[2];
    bool containsKey(int bit) { return links[bit] != nullptr; }
    void put(int bit, Node* node) { links[bit] = node; }
    Node* get(int bit) { return links[bit]; }
};

class Trie {
    Node* root;
public:
    Trie() { root = new Node(); }
    void insert(int num) {
        Node* node = root;
        for (int i = 31; i >= 0; i--) {
            int bit = (num >> i) & 1;
            if (!node->containsKey(bit)) node->put(bit, new Node());
            node = node->get(bit);
        }
    }
    int getMax(int num) {
        if(!root->containsKey(0) && !root->containsKey(1)) return -1;
        Node* node = root;
        int maxNum = 0;
        for (int i = 31; i >= 0; i--) {
            int bit = (num >> i) & 1;
            if (node->containsKey(1 - bit)) {
                maxNum = maxNum | (1 << i);
                node = node->get(1 - bit);
            } else {
                node = node->get(bit);
            }
        }
        return maxNum;
    }
};

vector<int> maximizeXor(vector<int>& nums, vector<vector<int>>& queries) {
    sort(nums.begin(), nums.end());
    vector<pair<int, pair<int, int>>> oQ;
    for (int i = 0; i < queries.size(); i++) {
        oQ.push_back({queries[i][1], {queries[i][0], i}});
    }
    sort(oQ.begin(), oQ.end());
    
    vector<int> ans(queries.size(), 0);
    int ind = 0;
    int n = nums.size();
    Trie trie;
    
    for (int i = 0; i < oQ.size(); i++) {
        int m = oQ[i].first;
        int x = oQ[i].second.first;
        int qInd = oQ[i].second.second;
        
        while (ind < n && nums[ind] <= m) {
            trie.insert(nums[ind]);
            ind++;
        }
        ans[qInd] = trie.getMax(x);
    }
    return ans;
}
```

## New Keywords / STL Used
Offline queries, `pair` sorting

## Edge Cases
No array elements are $\le m$ (returns -1).
