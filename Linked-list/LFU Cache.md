---
type: concept
tags: [linked-list, cpp, design, lfu]
date: 2026-06-30
---
# LFU Cache

## Problem Statement
Design and implement a data structure mapping to a Least Frequently Used (LFU) cache maintaining O(1) `get` and `put` operational bounds.

## Approach / Intuition
Establish two synchronized hash maps: one linking explicit keys directly to nodes containing values and frequencies, and another directly mapping standard frequencies to doubly linked lists of active nodes. Tracking the absolute minimal active frequency instantly identifies candidates for eviction relying on intensive [[Frequency Tracking]].

## Time & Space Complexity
- **[[Time Complexity]]:** O(1)
- **[[Space Complexity]]:** O(Capacity)

## Sample Code
```cpp
class LFUCache {
    struct Node {
        int key, val, freq;
        Node(int k, int v) : key(k), val(v), freq(1) {}
    };
    int capacity, minFreq;
    unordered_map<int, Node*> keyNode;
    unordered_map<int, list<Node*>> freqList;
    unordered_map<int, list<Node*>::iterator> pos;

public:
    LFUCache(int cap) : capacity(cap), minFreq(0) {}

    int get(int key) {
        if (!keyNode.count(key)) return -1;
        Node* node = keyNode[key];
        freqList[node->freq].erase(pos[key]);
        if (freqList[node->freq].empty() && minFreq == node->freq) {
            minFreq++;
        }
        node->freq++;
        freqList[node->freq].push_front(node);
        pos[key] = freqList[node->freq].begin();
        return node->val;
    }

    void put(int key, int value) {
        if (capacity == 0) return;
        if (keyNode.count(key)) {
            keyNode[key]->val = value;
            get(key);
            return;
        }
        if (keyNode.size() == capacity) {
            Node* evict = freqList[minFreq].back();
            keyNode.erase(evict->key);
            pos.erase(evict->key);
            freqList[minFreq].pop_back();
            delete evict;
        }
        Node* node = new Node(key, value);
        keyNode[key] = node;
        minFreq = 1;
        freqList[1].push_front(node);
        pos[key] = freqList[1].begin();
    }
};
```

## New Keywords / STL Used
`std::list`

## Edge Cases
Cache constrained forcefully to 0 capacity, massive collisions of keys sharing identical lowest frequencies.
