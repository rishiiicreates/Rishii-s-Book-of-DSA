---
type: concept
tags: [dsa, cpp, trie, string]
date: 2026-06-30
---
# Palindrome Pairs

## Problem Statement
Given a list of unique words, find all pairs of indices `(i, j)` such that the concatenation of the two words `words[i] + words[j]` forms a palindrome.

## Approach / Intuition
For a word pair $(A, B)$ to form a palindrome, either $B$ is the reverse of $A$'s prefix and the rest of $A$ is a palindrome, or $A$ is the reverse of $B$'s suffix and the rest of $B$ is a palindrome. We can reverse all words and insert them into a [[Trie]]. For each word, traverse the Trie. A match occurs if we exhaust the Trie node (meaning a reversed word matched our prefix) and the remaining suffix of our current word is a palindrome.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N \cdot L^2)$ where $N$ is word count, $L$ is max word length
- **[[Space Complexity]]:** $O(N \cdot L)$ for the Trie

## Sample Code
```cpp
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

bool isPalindrome(const string& s, int i, int j) {
    while (i < j) {
        if (s[i++] != s[j--]) return false;
    }
    return true;
}

struct TrieNode {
    TrieNode* next[26] = {nullptr};
    int wordIdx = -1;
    vector<int> palPrefixes;
};

class Solution {
public:
    vector<vector<int>> palindromePairs(vector<string>& words) {
        TrieNode* root = new TrieNode();
        for (int i = 0; i < words.size(); i++) {
            string w = words[i];
            reverse(w.begin(), w.end());
            TrieNode* curr = root;
            for (int j = 0; j < w.length(); j++) {
                if (isPalindrome(w, j, w.length() - 1)) curr->palPrefixes.push_back(i);
                int c = w[j] - 'a';
                if (!curr->next[c]) curr->next[c] = new TrieNode();
                curr = curr->next[c];
            }
            curr->wordIdx = i;
            curr->palPrefixes.push_back(i);
        }
        
        vector<vector<int>> res;
        for (int i = 0; i < words.size(); i++) {
            string w = words[i];
            TrieNode* curr = root;
            for (int j = 0; j < w.length(); j++) {
                if (curr->wordIdx != -1 && curr->wordIdx != i && isPalindrome(w, j, w.length() - 1)) {
                    res.push_back({i, curr->wordIdx});
                }
                curr = curr->next[w[j] - 'a'];
                if (!curr) break;
            }
            if (curr) {
                for (int j : curr->palPrefixes) {
                    if (i != j) res.push_back({i, j});
                }
            }
        }
        return res;
    }
};
```

## New Keywords / STL Used
`reverse`, indexing word matching

## Edge Cases
Empty strings in the list, words forming palindromes with themselves (avoid returning `(i, i)`).
