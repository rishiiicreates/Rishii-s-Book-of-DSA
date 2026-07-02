---
type: concept
tags: [sliding window, cpp, string-manipulation]
date: 2026-06-30
---
# Count Occurrences of Anagrams

## Problem Statement
Given a word and a text, return the count of all anagrams of the word in the text.

## Approach / Intuition
Maintain a frequency map of the word's characters. Use a fixed-size [[Sliding Window]] of length equal to the word's length over the text. Track the number of characters that fully match their required frequency. As the window slides, add the new character on the right and remove the old character from the left, updating the match count. If all distinct characters match, an anagram is found.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(|text|)$
- **[[Space Complexity]]:** $O(1)$ (constant size alphabet map)

## Sample Code
```cpp
#include <string>
#include <vector>

using namespace std;

int search(string pat, string txt) {
    vector<int> map(256, 0);
    for (char c : pat) map[c]++;
    
    int count = 0;
    for (int i = 0; i < 256; i++) {
        if (map[i] > 0) count++;
    }
    
    int i = 0, j = 0, ans = 0;
    while (j < txt.length()) {
        map[txt[j]]--;
        if (map[txt[j]] == 0) count--;
        
        if (j - i + 1 < pat.length()) {
            j++;
        } else if (j - i + 1 == pat.length()) {
            if (count == 0) ans++;
            if (map[txt[i]] == 0) count++;
            map[txt[i]]++;
            i++;
            j++;
        }
    }
    return ans;
}
```

## New Keywords / STL Used
Fixed-size window tracking

## Edge Cases
Pattern length greater than text length, no anagrams exist, string and pattern are identical.
