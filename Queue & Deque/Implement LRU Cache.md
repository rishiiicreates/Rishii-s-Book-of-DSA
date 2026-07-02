---
type: concept
tags: [queue, deque, cpp, cache]
date: 2026-06-30
---
# Implement LRU Cache

## Problem Statement
Design and implement a data structure for Least Recently Used (LRU) cache, supporting `get` and `put` in O(1) average time complexity.

## Approach / Intuition
Use a combination of an `unordered_map` and a doubly [[Linked List]] (or `std::list`). The hash map provides O(1) access to nodes. The linked list maintains the usage order, with the most recently used at the front and the least recently used at the back. Every time a node is accessed or updated, move it to the front.

## Time & Space Complexity
- **[[Time Complexity]]:** O(1) for both get and put
- **[[Space Complexity]]:** O(capacity)

## Sample Code
```cpp
class LRUCache {
    int cap;
    list<pair<int, int>> dll;
    unordered_map<int, list<pair<int, int>>::iterator> cache;
public:
    LRUCache(int capacity) : cap(capacity) {}
    
    int get(int key) {
        if (cache.find(key) == cache.end()) return -1;
        dll.splice(dll.begin(), dll, cache[key]);
        return cache[key]->second;
    }
    
    void put(int key, int value) {
        if (cache.find(key) != cache.end()) {
            cache[key]->second = value;
            dll.splice(dll.begin(), dll, cache[key]);
            return;
        }
        if (dll.size() == cap) {
            cache.erase(dll.back().first);
            dll.pop_back();
        }
        dll.push_front({key, value});
        cache[key] = dll.begin();
    }
};
```

## New Keywords / STL Used
`std::list`, `std::list::splice`, `std::unordered_map`

## Edge Cases
Cache capacity is 1, updating an existing key without exceeding capacity.
