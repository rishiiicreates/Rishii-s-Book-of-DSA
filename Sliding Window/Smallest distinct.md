---
type: concept
tags: [sliding window, cpp, string-manipulation]
date: 2026-06-30
---
# Smallest Window Containing All Distinct Characters

## Problem Statement
Given a string, find the smallest window length that contains all the distinct characters of the given string.

## Approach / Intuition
First, determine the total number of distinct characters in the [[String]] using a set or frequency array. Then, apply the [[Sliding Window]] technique. Expand the window by adding characters to a current frequency map until the number of distinct characters in the window matches the total distinct count. Once matched, shrink the window from the left to minimize its length while maintaining all distinct characters.

## Time & Space Complexity
- **[[Time Complexity]]:** $O(N)$
- **[[Space Complexity]]:** $O(1)$ (alphabet size)

## Sample Code
```cpp
#include <string>
#include <vector>
#include <unordered_set>
#include <algorithm>

using namespace std;

int findSubString(string str) {
    unordered_set<char> distinct_chars(str.begin(), str.end());
    int unique_count = distinct_chars.size();
    
    vector<int> count(256, 0);
    int start = 0, start_index = -1, min_len = 1e9;
    int curr_count = 0;
    
    for (int j = 0; j < str.length(); j++) {
        count[str[j]]++;
        if (count[str[j]] == 1) curr_count++;
        
        if (curr_count == unique_count) {
            while (count[str[start]] > 1) {
                count[str[start]]--;
                start++;
            }
            int len_window = j - start + 1;
            if (min_len > len_window) {
                min_len = len_window;
                start_index = start;
            }
        }
    }
    return min_len;
}
```

## New Keywords / STL Used
`unordered_set`

## Edge Cases
String with all same characters, empty string, string with all unique characters.
