---
type: concept
tags: [cpp]
date: 2026-06-30
---
# Smallest containing 0, 1 and 2

## Problem Statement
Given a string containing only '0', '1', and '2', find the length of the smallest substring that contains all three characters.

## Approach / Intuition
This can be efficiently solved using a [[Sliding Window]] approach. We can simply iterate through the string while keeping track of the most recent indices where '0', '1', and '2' were seen. Whenever we have valid indices for all three characters, the length of the substring ending at the current position is `current_index - minimum_of_the_three_indices + 1`. We take the minimum of these valid lengths across the entire traversal.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N)
- **[[Space Complexity]]:** O(1)

## Sample Code
```cpp
#include <string>
#include <algorithm>
#include <climits>

using namespace std;

int smallestSubstring(string S) {
    int last0 = -1, last1 = -1, last2 = -1;
    int min_len = INT_MAX;
    
    for (int i = 0; i < S.length(); ++i) {
        if (S[i] == '0') last0 = i;
        else if (S[i] == '1') last1 = i;
        else if (S[i] == '2') last2 = i;
        
        if (last0 != -1 && last1 != -1 && last2 != -1) {
            int current_len = i - min({last0, last1, last2}) + 1;
            min_len = min(min_len, current_len);
        }
    }
    
    return min_len == INT_MAX ? -1 : min_len;
}
```

## New Keywords / STL Used
`std::min` with initializer list, `INT_MAX`

## Edge Cases
- String shorter than 3 characters
- String missing one or more of the required characters
- Substrings repeating characters multiple times before containing all three
