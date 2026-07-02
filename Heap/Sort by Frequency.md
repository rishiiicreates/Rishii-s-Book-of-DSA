---
type: concept
tags: [heap, dsa, cpp]
date: 2026-06-30
---
# Sort by Frequency

## Problem Statement
Sort the characters in a string in decreasing order based on their frequency of occurrence.

## Approach / Intuition
First, compute the frequency of each character using a Hash Map. Then, push the character-frequency pairs into a max-[[Heap]]. Because the max-heap orders elements by the first value (which we make the frequency), we can simply pop characters from the heap and append them to the result string according to their frequency until the heap is empty.

## Time & Space Complexity
- **[[Time Complexity]]:** O(N + U log U) where N is the length of the string and U is the number of unique characters.
- **[[Space Complexity]]:** O(U) for the hash map and the heap.

## Sample Code
```cpp
#include <string>
#include <unordered_map>
#include <queue>

using namespace std;

string frequencySort(string s) {
    unordered_map<char, int> freq;
    for (char c : s) {
        freq[c]++;
    }
    
    priority_queue<pair<int, char>> maxHeap;
    for (auto& it : freq) {
        maxHeap.push({it.second, it.first});
    }
    
    string result = "";
    while (!maxHeap.empty()) {
        auto top = maxHeap.top();
        maxHeap.pop();
        result.append(top.first, top.second);
    }
    
    return result;
}
```

## New Keywords / STL Used
`std::unordered_map`, `std::priority_queue`, `std::pair`, `string::append`

## Edge Cases
- String with all unique characters.
- String with all identical characters.
- Empty string.
