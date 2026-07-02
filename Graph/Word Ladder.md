---
type: concept
tags: [graph, cpp, bfs, shortest-path]
date: 2026-06-30
---
# Word Ladder

## Problem Statement
Find the length of the shortest transformation sequence from a begin word to an end word, changing one letter at a time, with all intermediate words in a given dictionary.

## Approach / Intuition
Perform [[BFS]] starting from the begin word. In each step, change each character of the current word from 'a' to 'z' and check if the new word is in the dictionary to proceed level by level.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N \times M \times 26) where N is number of words, M is word length.
- **[[Space Complexity]]:** O(N)

## Sample Code
```cpp
int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
    unordered_set<string> dict(wordList.begin(), wordList.end());
    if (dict.find(endWord) == dict.end()) return 0;
    queue<pair<string, int>> q;
    q.push({beginWord, 1});
    while (!q.empty()) {
        string word = q.front().first;
        int steps = q.front().second;
        q.pop();
        if (word == endWord) return steps;
        for (int i = 0; i < word.size(); i++) {
            char original = word[i];
            for (char ch = 'a'; ch <= 'z'; ch++) {
                word[i] = ch;
                if (dict.find(word) != dict.end()) {
                    dict.erase(word);
                    q.push({word, steps + 1});
                }
            }
            word[i] = original;
        }
    }
    return 0;
}
```

## New Keywords / STL Used
`unordered_set`, `queue`

## Edge Cases
End word not in dictionary, begin word and end word are same, impossible transformation.
