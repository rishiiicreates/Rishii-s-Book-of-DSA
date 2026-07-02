---
type: concept
tags: [greedy, cpp, no-two-adjacent-same]
date: 2026-06-30
---
# No Two Adjacent are Same

## Problem Statement
Given a string of characters, rearrange the characters so that no two adjacent characters are the same. If it's impossible, return an empty string.

## Approach / Intuition
Count character frequencies. Use a max-heap ([[Priority Queue]]) to greedily place the most frequent remaining character. Keep track of the previously placed character to avoid adjacency, pushing it back into the heap only after the next character is chosen using a [[Greedy Algorithm]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(N log K) where K is distinct characters
- **[[Space Complexity]]:** O(K)

## Sample Code
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <queue>
#include <unordered_map>

using namespace std;

string reorganizeString(string s) {
    unordered_map<char, int> count;
    for (char c : s) count[c]++;
    priority_queue<pair<int, char>> pq;
    for (auto& p : count) pq.push({p.second, p.first});
    
    string res = "";
    pair<int, char> prev = {-1, '#'};
    while (!pq.empty()) {
        auto current = pq.top(); pq.pop();
        res += current.second;
        if (prev.first > 0) pq.push(prev);
        current.first--;
        prev = current;
    }
    if (res.length() != s.length()) return "";
    return res;
}
```

## New Keywords / STL Used
- `unordered_map`, `priority_queue` with `pair`

## Edge Cases
- Impossible configurations (e.g., "aaab")
- Already valid string
