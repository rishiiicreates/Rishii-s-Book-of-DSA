---
type: concept
tags: [queue, deque, cpp, implementation]
date: 2026-06-30
---
# Implement Deque Using Array or Linked List

## Problem Statement
Implement a Double Ended Queue (Deque) from scratch.

## Approach / Intuition
A [[Deque]] can be effectively built using a Doubly [[Linked List]] to allow O(1) insertions and deletions at both the head and the tail. Alternatively, a circular array with `front` and `rear` pointers can be used, taking care of modulo arithmetic for boundary wrapping on both ends.

## Time & Space Complexity
- **[[Time Complexity]]:** O(1) for all push/pop operations
- **[[Space Complexity]]:** O(n)

## Sample Code
```cpp
struct DequeNode {
    int val;
    DequeNode *prev, *next;
    DequeNode(int x) : val(x), prev(nullptr), next(nullptr) {}
};

class MyDeque {
    DequeNode *head, *tail;
public:
    MyDeque() : head(nullptr), tail(nullptr) {}
    
    void push_back(int val) {
        DequeNode* node = new DequeNode(val);
        if (!tail) { head = tail = node; } 
        else { node->prev = tail; tail->next = node; tail = node; }
    }
    
    void pop_front() {
        if (!head) return;
        DequeNode* temp = head;
        head = head->next;
        if (head) head->prev = nullptr;
        else tail = nullptr;
        delete temp;
    }
};
```

## New Keywords / STL Used
None specific

## Edge Cases
Popping from both ends until empty, pushing and popping single elements repeatedly.
