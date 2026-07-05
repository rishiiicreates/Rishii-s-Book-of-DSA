# Duplicates in Binary Matrix

## Problem Statement
- given a binary [[Matrix]], find and return all the duplicate rows.

## Approach / Intuition
- insert each row into a [[Trie]] where nodes represent $0$ or $1$. If traversing a row reaches an existing end-of-word node, it is a duplicate. Otherwise, mark the end node to track it for future rows.

## Time & Space Complexity
- **[[time Complexity]]:** $O(R \times C)$
- **[[space Complexity]]:** $O(R \times C)$

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
- none

## Edge Cases
- empty matrix, no duplicates.

NEXT: [[Index]]
