# Search Suggestions System

## Problem Statement
- given an array of strings `products` and a `searchWord`, design a system that suggests at most three product names from `products` after each character of `searchWord` is typed.

## Approach / Intuition
- sort the products lexicographically. Insert them into a [[Trie]], where each node stores a list of up to three product indices (or the strings themselves) that share the prefix ending at that node. When typing characters, traverse the Trie; at each node, output the stored suggestions. Alternatively, this can be solved using two-pointers or [[Binary Search]] without a Trie, but a Trie explicitly models the prefix matching structure.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N \log N + N \cdot L + Q)$ where $N$ is product count, $L$ is max length, $Q$ is searchWord length
- **[[space Complexity]]:** $O(N \cdot L)$ for Trie

## Sample Code
```cpp
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

struct TrieNode {
    TrieNode* children[26] = {nullptr};
    vector<int> indices;
};

class Solution {
public:
    vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) {
        sort(products.begin(), products.end());
        TrieNode* root = new TrieNode();
        
        for (int i = 0; i < products.size(); i++) {
            TrieNode* curr = root;
            for (char c : products[i]) {
                if (!curr->children[c - 'a']) {
                    curr->children[c - 'a'] = new TrieNode();
                }
                curr = curr->children[c - 'a'];
                if (curr->indices.size() < 3) {
                    curr->indices.push_back(i);
                }
            }
        }
        
        vector<vector<string>> result;
        TrieNode* curr = root;
        bool notFound = false;
        
        for (char c : searchWord) {
            if (notFound || !curr->children[c - 'a']) {
                notFound = true;
                result.push_back({});
            } else {
                curr = curr->children[c - 'a'];
                vector<string> suggestions;
                for (int idx : curr->indices) {
                    suggestions.push_back(products[idx]);
                }
                result.push_back(suggestions);
            }
        }
        
        return result;
    }
};
```

## New Keywords / STL Used
- vector accumulation inside struct

## Edge Cases
- no products match the search word, less than three products match.

NEXT: [[Index]]
