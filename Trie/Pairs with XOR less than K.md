# Count Pairs With XOR in a Range

## Problem Statement
- given an integer array and a boundary limit, count how many pairs have an XOR sum strictly less than a specific value.

## Approach / Intuition
- use a binary [[Trie]] where each node maintains a `count` of how many numbers pass through it. For each number $x$ in the array, compute how many previously inserted numbers have an XOR with $x$ that is strictly less than a limit $L$. Traverse the Trie bit by bit. Depending on the bits of $x$ and the limit $L$, either add the entire `count` of a subtree or traverse deeper.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N \cdot 15)$ (assuming max values fit in 15 bits)
- **[[space Complexity]]:** $O(N \cdot 15)$

## Sample Code
```cpp
#include <vector>

using namespace std;

struct TrieNode {
    TrieNode* child[2];
    int count;
    TrieNode() {
        child[0] = child[1] = nullptr;
        count = 0;
    }
};

class Trie {
public:
    TrieNode* root;
    Trie() { root = new TrieNode(); }
    
    void insert(int val) {
        TrieNode* curr = root;
        for (int i = 14; i >= 0; i--) {
            int bit = (val >> i) & 1;
            if (!curr->child[bit]) curr->child[bit] = new TrieNode();
            curr = curr->child[bit];
            curr->count++;
        }
    }
    
    int query(int val, int limit) {
        TrieNode* curr = root;
        int ans = 0;
        for (int i = 14; i >= 0 && curr; i--) {
            int bit_val = (val >> i) & 1;
            int bit_limit = (limit >> i) & 1;
            
            if (bit_limit == 1) {
                if (curr->child[bit_val]) ans += curr->child[bit_val]->count;
                curr = curr->child[1 - bit_val];
            } else {
                curr = curr->child[bit_val];
            }
        }
        return ans;
    }
};

int countPairs(vector<int>& nums, int low, int high) {
    Trie trie;
    int count = 0;
    for (int num : nums) {
        count += (trie.query(num, high + 1) - trie.query(num, low));
        trie.insert(num);
    }
    return count;
}
```

## New Keywords / STL Used
- subtree counting

## Edge Cases
- empty array, small array limits.

NEXT: [[Index]]
