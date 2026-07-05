# Longest Word With All Prefixes

## Problem Statement
- find the longest string in an array such that every prefix of it is also in the array. If there is a tie, return the lexicographically smallest.

## Approach / Intuition
- insert all words into a [[Trie]]. Then, check each word character by character to see if every prefix (node) has its `isEnd` flag set. Keep track of the longest valid word.

## Time & Space Complexity
- **[[time Complexity]]:** $O(\sum L)$ to build and search
- **[[space Complexity]]:** $O(\sum L \cdot \Sigma)$

## Sample Code
```cpp
class TrieNode {
public:
    TrieNode* children[26];
    bool isEnd = false;
    TrieNode() {
        for (int i = 0; i < 26; i++) children[i] = nullptr;
    }
};

class Trie {
public:
    TrieNode* root = new TrieNode();
    
    void insert(const string& word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->children[c - 'a']) {
                node->children[c - 'a'] = new TrieNode();
            }
            node = node->children[c - 'a'];
        }
        node->isEnd = true;
    }
    
    bool checkPrefixes(const string& word) {
        TrieNode* node = root;
        for (char c : word) {
            node = node->children[c - 'a'];
            if (!node->isEnd) return false;
        }
        return true;
    }
};

string longestWord(vector<string>& words) {
    Trie trie;
    for (const string& w : words) trie.insert(w);
    
    string result = "";
    for (const string& w : words) {
        if (trie.checkPrefixes(w)) {
            if (w.length() > result.length() || (w.length() == result.length() && w < result)) {
                result = w;
            }
        }
    }
    return result;
}
```

## New Keywords / STL Used
- none

## Edge Cases
- empty array, no word with all prefixes (returns empty string).

NEXT: [[Index]]
