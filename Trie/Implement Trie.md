# Implement Trie

## Problem Statement
- implement a Trie (Prefix Tree) with `insert`, `search`, and `startsWith` methods.

## Approach / Intuition
- create a `TrieNode` structure with an array of children pointers (usually size 26 for lowercase English letters) and a boolean flag indicating the end of a word. Traverse down the [[Tree Data Structure]], creating nodes if they don't exist.

## Time & Space Complexity
- **[[time Complexity]]:** $O(L)$ for insert/search where $L$ is word length
- **[[space Complexity]]:** $O(N \cdot L \cdot \Sigma)$ where $N$ is number of words, $\Sigma$ is alphabet size

## Sample Code
```cpp
class TrieNode {
public:
    TrieNode* children[26];
    bool isEnd;
    TrieNode() {
        isEnd = false;
        for (int i = 0; i < 26; i++) {
            children[i] = nullptr;
        }
    }
};

class Trie {
    TrieNode* root;
public:
    Trie() {
        root = new TrieNode();
    }
    
    void insert(string word) {
        TrieNode* node = root;
        for (char c : word) {
            int idx = c - 'a';
            if (!node->children[idx]) {
                node->children[idx] = new TrieNode();
            }
            node = node->children[idx];
        }
        node->isEnd = true;
    }
    
    bool search(string word) {
        TrieNode* node = root;
        for (char c : word) {
            int idx = c - 'a';
            if (!node->children[idx]) return false;
            node = node->children[idx];
        }
        return node->isEnd;
    }
    
    bool startsWith(string prefix) {
        TrieNode* node = root;
        for (char c : prefix) {
            int idx = c - 'a';
            if (!node->children[idx]) return false;
            node = node->children[idx];
        }
        return true;
    }
};
```

## New Keywords / STL Used
- `nullptr`

## Edge Cases
- empty string operations, overlapping prefixes.

NEXT: [[Index]]
