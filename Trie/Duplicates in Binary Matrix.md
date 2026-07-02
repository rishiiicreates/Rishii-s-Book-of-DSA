---
type: concept
tags: [cpp, trie]
date: 2026-06-30
---
# Duplicates in Binary Matrix

## Problem Statement
Given a binary matrix, find and return all the duplicate rows.

## Approach / Intuition
Insert each row into a [[Trie]] where nodes represent $0$ or $1$. If traversing a row reaches an existing end-of-word node, it is a duplicate. Otherwise, mark the end node to track it for future rows.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(R \times C)$
- **[[Space Complexity]]:** $O(R \times C)$

## Sample Code
```cpp
class TrieNode {
public:
    TrieNode* children[2];
    bool isEnd;
    TrieNode() {
        children[0] = nullptr;
        children[1] = nullptr;
        isEnd = false;
    }
};

vector<int> findDuplicateRows(vector<vector<int>>& matrix) {
    TrieNode* root = new TrieNode();
    vector<int> duplicates;
    int r = matrix.size();
    if (r == 0) return duplicates;
    int c = matrix[0].size();
    
    for (int i = 0; i < r; i++) {
        TrieNode* node = root;
        for (int j = 0; j < c; j++) {
            int bit = matrix[i][j];
            if (!node->children[bit]) {
                node->children[bit] = new TrieNode();
            }
            node = node->children[bit];
        }
        if (node->isEnd) {
            duplicates.push_back(i);
        } else {
            node->isEnd = true;
        }
    }
    return duplicates;
}
```

## New Keywords / STL Used
None

## Edge Cases
Empty matrix, no duplicates.
