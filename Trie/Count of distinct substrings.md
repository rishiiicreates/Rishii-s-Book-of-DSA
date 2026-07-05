# Count of distinct substrings

## Problem Statement
- given a string, find the number of distinct substrings it contains.

## Approach / Intuition
- insert every suffix of the string into a [[Trie]]. The number of distinct substrings is equal to the number of nodes in the Trie (excluding the root). Each new node created represents a new distinct substring.

## Time & Space Complexity
- **[[time Complexity]]:** $O(N^2)$ where $N$ is string length
- **[[space Complexity]]:** $O(N^2)$

## Sample Code
```cpp
class TrieNode {
public:
    TrieNode* children[26];
    TrieNode() {
        for (int i = 0; i < 26; i++) children[i] = nullptr;
    }
};

int countDistinctSubstrings(string s) {
    TrieNode* root = new TrieNode();
    int count = 0;
    int n = s.length();
    
    for (int i = 0; i < n; i++) {
        TrieNode* node = root;
        for (int j = i; j < n; j++) {
            int idx = s[j] - 'a';
            if (!node->children[idx]) {
                node->children[idx] = new TrieNode();
                count++;
            }
            node = node->children[idx];
        }
    }
    return count + 1;
}
```

## New Keywords / STL Used
- none

## Edge Cases
- string with identical characters, empty string.

NEXT: [[Index]]
