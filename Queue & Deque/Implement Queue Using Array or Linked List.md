---
type: concept
tags: [queue, deque, cpp, implementation]
date: 2026-06-30
---
# Implement Queue Using Array or Linked List

## Problem Statement
Implement a custom queue data structure from scratch using either a static array with a circular behavior or a singly linked list.

## Approach / Intuition
For an array implementation, maintain `front` and `rear` pointers, using modulo arithmetic for circular indexing to reuse space. For a [[Linked List]] implementation, keep pointers to both the `head` (for popping) and `tail` (for pushing) to ensure O(1) operations.

## Time & Space Complexity
- **[[Time Complexity]]:** O(1) for enqueue and dequeue
- **[[Space Complexity]]:** O(n)

## Sample Code
```cpp
class QueueNode {
public:
    int data;
    QueueNode* next;
    QueueNode(int val) : data(val), next(nullptr) {}
};

class MyQueue {
    QueueNode *front, *rear;
public:
    MyQueue() : front(nullptr), rear(nullptr) {}
    
    void push(int val) {
        QueueNode* temp = new QueueNode(val);
        if (rear == nullptr) {
            front = rear = temp;
            return;
        }
        rear->next = temp;
        rear = temp;
    }
    
    void pop() {
        if (front == nullptr) return;
        QueueNode* temp = front;
        front = front->next;
        if (front == nullptr) rear = nullptr;
        delete temp;
    }
};
```

## New Keywords / STL Used
`new`, `delete`

## Edge Cases
Popping when empty, pushing after pop to empty state.
