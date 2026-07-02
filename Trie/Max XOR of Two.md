---
type: concept
tags: [dsa, cpp, trie, bit-manipulation]
date: 2026-06-30
---
# Maximum XOR of Two Numbers in an Array

## Problem Statement
Given an array of integers, find the maximum XOR value of any two elements in the array.

## Approach / Intuition
Use a binary [[Trie]] to store the binary representations of the numbers (starting from the most significant bit). For each number in the array, traverse the Trie attempting to maximize the XOR. At each bit level, check if the opposite bit exists in the Trie (to yield a 1 in XOR). If it does, follow that path; otherwise, follow the same bit path. This combines [[Bit Manipulation]] with a specialized Trie.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N \cdot 32)$
- **[[Space Complexity]]:** $O(N \cdot 32)$

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
private:
    Node* root;
public:
    Trie() { root = new Node(); }
    
    void insert(int num) {
        Node* node = root;
        for (int i = 31; i >= 0; i--) {
            int bit = (num >> i) & 1;
            if (!node->containsKey(bit)) {
                node->put(bit, new Node());
            }
            node = node->get(bit);
        }
    }
    
    int getMax(int num) {
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

int findMaximumXOR(vector<int>& nums) {
    Trie trie;
    for (int num : nums) trie.insert(num);
    int maxi = 0;
    for (int num : nums) {
        maxi = max(maxi, trie.getMax(num));
    }
    return maxi;
}
```

## New Keywords / STL Used
Bitwise shift, bitwise AND/OR

## Edge Cases
Array of size 1 (returns 0), array with all identical elements.
