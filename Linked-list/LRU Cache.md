---
type: concept
tags: [linked-list, cpp, design, lru]
date: 2026-06-30
---
# LRU Cache

## Problem Statement
Design a data structure that fundamentally follows the constraints of a Least Recently Used (LRU) cache. It must actively support `get` and `put` operations strictly in O(1) average time complexity.

## Approach / Intuition
Pair a hash map structurally with a doubly linked list. The hash map ensures reliable O(1) node access by key, while the doubly linked list inherently supports rapid O(1) removals and constant insertions at the head, strictly maintaining the internal [[Eviction Policy]] order flawlessly.

## Time & Space Complexity
- **[[Time Complexity]]:** O(1)
- **[[Space Complexity]]:** O(Capacity)

## Sample Code
```cpp
class LRUCache {
    struct Node {
        int key, val;
        Node* prev;
        Node* next;
        Node(int k, int v) : key(k), val(v), prev(nullptr), next(nullptr) {}
    };
    int capacity;
    unordered_map<int, Node*> cache;
    Node* head;
    Node* tail;
    
    void remove(Node* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }
    void insert(Node* node) {
        node->next = head->next;
        node->prev = head;
        head->next->prev = node;
        head->next = node;
    }
public:
    LRUCache(int cap) : capacity(cap) {
        head = new Node(-1, -1);
        tail = new Node(-1, -1);
        head->next = tail;
        tail->prev = head;
    }
    int get(int key) {
        if (cache.find(key) == cache.end()) return -1;
        Node* node = cache[key];
        remove(node);
        insert(node);
        return node->val;
    }
    void put(int key, int value) {
        if (cache.find(key) != cache.end()) {
            remove(cache[key]);
        }
        if (cache.size() == capacity) {
            cache.erase(tail->prev->key);
            remove(tail->prev);
        }
        Node* newNode = new Node(key, value);
        insert(newNode);
        cache[key] = newNode;
    }
};
```

## New Keywords / STL Used
`struct`

## Edge Cases
Cache accessed heavily with non-existent keys, initialization with a rigid capacity of exactly 1.
