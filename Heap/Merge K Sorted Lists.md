---
type: concept
tags: [heap, priority-queue, linked-list, divide-and-conquer, cpp, math]
date: 2026-06-30
---
# Merge K Sorted Lists

## Problem Statement
Given an array of $K$ discrete, monotonically sorted linked lists, mathematically amalgamate the entire collective structure into one singular, universally monotonically sorted linked list. The geometric topology dictates the unified list must maintain strict monotonic continuity bounding every physical scalar originating from the absolute set of $K$ sub-geometries.

---

## Approach: K-Way Min-Heap Temporal Extraction

Naively fusing geometries consecutively linearly incurs absolute $\mathcal{O}(N \cdot K)$ temporal bounds due to recursive subset sweeping. Instead, we algebraically leverage a strict invariant: the absolute global minimum scalar fundamentally must permanently currently reside at one of the $K$ topological list heads.

By mapping the temporal heads natively into an active physical geometric Min-Heap (Priority Queue) bounded absolutely by size $K$:
1. Initialization mathematically inserts exactly one pointer bounding each discrete list ($K$ elements) directly into the Min-Heap.
2. Iterative Sweep:
   - Temporally extract the absolute minimal scalar pointer structurally residing at the geometric apex.
   - Project and bind this node explicitly onto the continuous accumulating universal sequence geometry.
   - If the dynamically evaluated node contains a topologically valid subsequent child physically bounded within its original native list (`node->next != nullptr`), algebraically push the child bounds directly into the Min-Heap.
3. The geometric structure strictly sustains absolute size $K$, implicitly evaluating bounding minima in strict $\mathcal{O}(\log K)$ logarithmic geometry.

---

## Code Implementation

```cpp
#include <vector>
#include <queue>

using namespace std;

struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

// Structural Functor enforcing strict ascending Min-Heap topological comparator
struct CompareNode {
    bool operator()(ListNode* const& a, ListNode* const& b) const {
        return a->val > b->val; // Invert native max-heap bounding
    }
};

ListNode* mergeKLists(vector<ListNode*>& lists) {
    // Geometrically bounded Min-Heap restricted purely to active list pointers
    priority_queue<ListNode*, vector<ListNode*>, CompareNode> min_heap;
    
    // 1. Initialize topological invariant subset
    for (ListNode* head : lists) {
        if (head != nullptr) {
            min_heap.push(head);
        }
    }
    
    // Abstract temporal anchor maintaining sequence initiation geometry
    ListNode dummy(0);
    ListNode* tail = &dummy;
    
    // 2. Iterative strict geometric minimum extraction and subsequent subset projection
    while (!min_heap.empty()) {
        ListNode* min_node = min_heap.top();
        min_heap.pop();
        
        // Append absolute mathematical minimum to continuous bounds
        tail->next = min_node;
        tail = tail->next;
        
        // Advance topological bound natively propagating the extracted sequence list
        if (min_node->next != nullptr) {
            min_heap.push(min_node->next);
        }
    }
    
    return dummy.next;
}
```

> [!tip]
> While a Min-Heap dynamically guarantees optimal bounds sequentially, an explicit Divide-and-Conquer geometric topological reduction equally mathematically merges sub-lists symmetrically achieving identical rigorous $\mathcal{O}(N \log K)$ theoretical boundaries devoid of implicit priority-queue structural overhead.

---

## Complexity Analysis
- **Time Complexity:** $\mathcal{O}(N \log K)$. Every absolute node natively enters and mathematically exits the strict $K$-bounded heap structure identically precisely once. $N$ represents the global sum of absolutely all elements.
- **Space Complexity:** $\mathcal{O}(K)$. The continuous Min-Heap physically explicitly maintains strictly up to absolutely $K$ localized geometric pointers bounding instantaneous limits.
