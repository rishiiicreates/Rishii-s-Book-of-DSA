---
type: concept
tags: [queue, deque, cpp, cache]
date: 2026-06-30
---
# Implement LFU Cache

## Problem Statement
Design and implement a data structure for Least Frequently Used (LFU) cache, supporting `get` and `put` in O(1) average time complexity.

## Approach / Intuition
Use two hash maps: one mapping keys to `{value, frequency}` and another mapping frequencies to a doubly [[Linked List]] of keys. Maintain a `minFreq` variable. On `get` or `put`, increment the frequency, move the key from its current frequency list to the `freq + 1` list, and update `minFreq` if the previous list becomes empty.

## Time & Space Complexity
- **[[Time Complexity]]:** O(1) for both get and put
- **[[Space Complexity]]:** O(capacity)

## Sample Code
```cpp
class LFUCache {
    int cap, minFreq;
    unordered_map<int, pair<int, int>> keyValFreq; 
    unordered_map<int, list<int>::iterator> keyIter; 
    unordered_map<int, list<int>> freqList; 
public:
    LFUCache(int capacity) : cap(capacity), minFreq(0) {}
    
    void updateFreq(int key) {
        int f = keyValFreq[key].second++;
        freqList[f].erase(keyIter[key]);
        if (freqList[f].empty() && minFreq == f) minFreq++;
        freqList[f + 1].push_front(key);
        keyIter[key] = freqList[f + 1].begin();
    }
    
    int get(int key) {
        if (keyValFreq.find(key) == keyValFreq.end()) return -1;
        updateFreq(key);
        return keyValFreq[key].first;
    }
    
    void put(int key, int value) {
        if (cap == 0) return;
        if (keyValFreq.find(key) != keyValFreq.end()) {
            keyValFreq[key].first = value;
            updateFreq(key);
            return;
        }
        if (keyValFreq.size() == cap) {
            int evictKey = freqList[minFreq].back();
            freqList[minFreq].pop_back();
            keyValFreq.erase(evictKey);
            keyIter.erase(evictKey);
        }
        minFreq = 1;
        freqList[1].push_front(key);
        keyValFreq[key] = {value, 1};
        keyIter[key] = freqList[1].begin();
    }
};
```

## New Keywords / STL Used
`std::list`, `std::unordered_map`

## Edge Cases
Capacity of 0, multiple items with the same minimum frequency (evict least recently used among them).
