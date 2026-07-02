---
type: concept
tags: [stack, cpp, linked-list, dynamic-memory]
date: 2026-07-01
---
# Stack Implementation using Linked List

## Problem Statement
Implement the Stack ADT with dynamic unbounded spatial capacity using a singly linked sequence mapping (Linked List).

---

## Approach: Head-Node Isomorphism

Unlike fixed arrays, linked structures allocate disjoint topological nodes on the heap domain dynamically. 
To satisfy the $O(1)$ temporal constraint for LIFO operations, the `Top` of the stack must structurally map identically to the `Head` of a Singly Linked List.

**Structural State Machine:**
1. **Node Topology:** Each element $x$ is encapsulated in a structure containing `data` and a memory pointer `next` pointing to the topologically inferior element.
2. **Push($x$):** 
   Create a new node $N$. Map $N \to \text{next} = \text{Head}$. Redefine $\text{Head} = N$. (This operates in strict $O(1)$ space and time mapping).
3. **Pop():** 
   Constraint: $\text{Head} \ne \text{Null}$.
   Define temp pointer $P = \text{Head}$. Redefine $\text{Head} = \text{Head} \to \text{next}$. Deallocate memory at $P$.
   
Since memory is allocated dynamically per element, the system is mathematically immune to Stack Overflow (until physical OS heap depletion).

---

## Code Implementation

```cpp
#include <iostream>
#include <stdexcept>

using namespace std;

// Topological Node Structure
struct Node {
    int data;
    Node* next;
    Node(int val) : data(val), next(nullptr) {}
};

class LinkedListStack {
private:
    Node* head; // Structurally isomorphic to the Stack Top

public:
    LinkedListStack() : head(nullptr) {}
    
    ~LinkedListStack() {
        while (!empty()) {
            pop();
        }
    }
    
    void push(int x) {
        Node* newNode = new Node(x);
        newNode->next = head;
        head = newNode;
    }
    
    void pop() {
        if (empty()) {
            throw underflow_error("Stack Underflow");
        }
        Node* temp = head;
        head = head->next;
        delete temp; // Strictly reclaim heap memory
    }
    
    int top() const {
        if (empty()) {
            throw underflow_error("Stack Underflow");
        }
        return head->data;
    }
    
    bool empty() const {
        return head == nullptr;
    }
};
```

---

## Complexity Analysis
- **Time Complexity:** $O(1)$ exact time for all LIFO operations.
- **Space Complexity:** $O(N)$ dynamically bounded to the exact element cardinality $N$.

> [!important]
> **Pointer Overhead:** While dynamically unbounded, every single element now incurs an $O(1)$ structural overhead (8 bytes on 64-bit architectures for the `next` pointer) plus potential heap fragmentation. Thus, while structurally elegant, a vector-backed stack is often vastly superior regarding CPU cache locality.
